$(document).ready(function() {
	$('#usn').on('input propertychange paste', function() {
		$('#dob').val('');
		$('#semester').hide();
		$('#usn_exist').text('');
	});
	$("#dob").keypress(function(event) {event.preventDefault();});
	var dob_picker = $('.datepicker').datepicker({
		format: "dd-mm-yyyy",
		autoclose:true
	})
	.on('changeDate', function(ev){
		if($('#usn').val() == ''){
			$('#usn_exist').text('Kindly Enter USN');
		}else if($('#dob').val() == ''){
			$('#dob_exist').text('Kindly Enter Date of Birth');
		}else{
			$('#dob_exist').text('');
			$('#semester').hide();
			var post_data = {
					'usn': $.trim($('#usn').val()),
					'dob': $.trim($('#dob').val())
			};
			$.ajax({
				type: "POST",
				url: base_url+"student_result/get_semester",	
				dataType:"json",
				data:post_data,
				success:function(html_value){
				if (html_value.length > 0) {	
						var i = 0;
						$('option:selected', "#semester").remove();
						for(key in html_value){
							var selected;
							if(i === 0){
								console.log(i);
								selected = 'selected = "selected"';
							}else{
								selected = '';
							}
							$("#semester").append("<option value=\"" + html_value[key].semester + "\" " + selected +">" + html_value[key].semester + "</option>");								
							i++;
						}
						$('#semester').show();
						$('#result_submit').removeAttr("disabled");
					}else{
						$('#result_submit').removeAttr("disabled");
					}
				}
			}); 
		}
		//$(this).datepicker('hide');
	  });
	var base_url = $('#base_url').val();
	$('#usn').on('focus',function(){
		$('#usn_exist').text('');
	});
	$('#dob').on('blur',function(){
		
	});
	$('#result_submit').on('click',function(e){ 
		
		if($('#usn').val() == '' ){
			$('#usn_exist').text('Kindly Enter USN');
		}else if($('#dob').val() == ''){
			$('#dob_exist').text('Kindly Enter Date of birth');
		}else{
		var post_data = {
				'usn': $.trim($('#usn').val()),
				'dob': $.trim($('#dob').val()),
				'semester': $.trim($('#semester').val())
		};
		$('#usn_value').val($.trim($('#usn').val()));	
		$.ajax({
			type: "POST",
			url: base_url+"student_result/get_results",	
			dataType:"json",
			data:post_data,
			success:function(html_value){
			if (html_value.length > 0) {					
					$('.login_screen').hide();
					$('#result_label').hide();
					$('#imp_notes').hide();
					if($('#allow_pdf').val() == 0){
						$('.pdf_allow').css('display','none');
						table_string = '</br>';
					}else{
						table_string = '';
					}
					table_string += '<table class="table table-bordered"  style="border: 1px solid #C1C1C1;border-collapse: separate !important;" width="100%"><thead><tr><th style="text-align: left;">Course Name</th><th>Course Code</th><th>Credits</th><th>CIE</th><th>SEE</th><th>Total Marks</th><th style="text-align: center;">Letter Grade</th><th style="text-align: center;">Credits Earned</th><th style="text-align: center;">Grade Points</th><th style="text-align: center;">Credit Points</th></tr></thead><tr>';
					var i = 1;
					$.each(html_value, function(){ 
					
					table_string += '<td width="40%" align="left" style="text-align: left;font-size:14px;">'+ this['crs_title'] +'</td>';
					table_string += '<td width="12%" align="center" style="font-size:14px;">'+this['crs_code'] +'</td>';			
					table_string += '<td width="6%" align="center">'+this['credits'] +'</td>';			
					table_string += '<td width="6%" align="center">'+this['total_cia'] +'</td>';			
					table_string += '<td width="6%" align="center">'+this['see'] +'</td>';			
					table_string += '<td width="6%" align="center">'+this['cia_see'] +'</td>';
					table_string += '<td width="6%" style="text-align: center;">'+this['grade']+'</td>';					
					table_string += '<td width="6%" style="text-align: center;">'+this['credits_earned']+'</td>';			
					
					table_string += '<td width="6%" style="text-align: center;">'+this['grade_point']+'</td>';
					table_string += '<td width="6%" style="text-align: center;">'+this['credits_earned'] * this['grade_point'] +'</td><tr>';
					$('#org_details').css('display','block');
					$('#stud_name').text(this['name']);
					$('#stud_usn').text(this['usno']);
					$('#stud_sem').text(this['semester']);
					$('#stud_pgm').text(this['pgm_title']);
					$('#stud_sgpa').text(this['sgpa']);
					$('#stud_cgpa').text(this['cgpa']);
					$('#result_year').text(this['result_year']);
					console.log(this['pgm_type']);
					if(this['pgm_type'] == 'UG'){
						$('.ug').show();
						$('.pg').hide();
					}else{
						$('.ug').hide();
						$('.pg').show();
					}
					i++;
					});
					table_string +='</table>';
					$("#result_body").removeClass("result_vw");
					$('#result_table').show();					
					$('#result_table').html(table_string);
					$('#footer_space').html('</br>');
				}else{
					$('#dob_exist').text('No Results Available');
				}
			}
		}); 
		}
	}); 
	
	// $("#usn").on('keypress', function (event) {
		// var keycode = (event.keyCode ? event.keyCode : event.which);
		// if (keycode == '13') {
			// event.preventDefault();
			// $('#result_submit').click();
		// }

	// });
	$('#print_result').on('click',function(){
		$('#usn_value').val($('#usn').val());
			$('#result_print_form').submit();
	});
});
function noBack() { 
window.history.forward(); 
}
$(window).on('beforeunload', function(e) {
	//e.preventDefault();
	window.location.href = $('#base_url').val()+"student_result";
});