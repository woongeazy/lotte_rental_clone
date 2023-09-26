/**
 * ==================================================================================
 * 폼 오브젝트 데이터 생성(자동)
 * ----------------------------------------------------------------------------------
 * data-array-always="true" 	: 데이터가 1개여도 무조건 배열 형태로 만들어 줌
 * data-date-format="format" 	: 선언한 데이트 형식으로 변경 함
 * 	format : yyyyMMdd
 * 	format : yyyy/MM/dd
 * 	format : yyyy-MM-dd
 * 	format : yyyy.MM.dd
 * data-valid-num 				: 데이터 형식이 1,234,567 -> 1234567로 변경하여 만들어 줌
 * data-check-only 				: 라디오 버튼일 경우 선언하면 체크한 값만 데이터로 반환 함
 * data-serialize-type="int"	: object 생성시 타입 정보
 * 	default : string
 * 	int/long/double
 * ----------------------------------------------------------------------------------
 * case
 * 	- input : hidden
 * 	- input : text
 *  - input : number
 *  - input : password
 *  - input : checkbox
 *  - select
 *  - textarea
 *  
 * case
 * 	- input : radio
 * ==================================================================================
 */
$.fn.serializeObject = function() {
	
	var o = {};
	var $frm = $(this);
	
	/** 
	 * ========================================================================================================
	 *  input type : hidden / text / number / password / checkbox
	 *  other : select / textarea
	 * ========================================================================================================
	 */
	$frm.find(
			'input[type=hidden], input[type=checkbox], input[type=tel], input[type=email],'+
			'input[type=number], input[type=password], input[type=text], select, textarea'
		).each(function(){
		
		if (this.name === null || this.name === undefined || this.name === '') {
			return;
		}
		
		if($(this).attr('data-serailize-none') !== undefined && $(this).attr('data-serailize-none') == 'true') {
			return;
		}
		
		var eleValue = null;
		
		// make array data
		if( $(this).attr('data-array-always') !== undefined && $(this).attr('data-array-always') == 'true' ) {
			
			// converter date format
			if($(this).attr('data-date-format') !== undefined) {
				if($frm.find('input[name='+this.name+']').length > 1) {
					eleValue = $(this).cvtDateFormat();
				}
				else {
					eleValue = [$(this).cvtDateFormat()];
				}
			}
			
			// converter number only (1,234,567 -> 1234567)
			else if( $(this).attr('data-valid-num') !== undefined || $(this).attr('data-valid-num') == 'true' ){
				if($frm.find('input[name='+this.name+']').length > 1) {
					eleValue = $(this).unComma();
				}
				else {
					eleValue = [$(this).unComma()];
				}
			}
			// other
			else {
				if($(this).is('select')) {
					if($frm.find('select[name='+this.name+']').length > 1) {
						eleValue = this.value;
					}
					else {
						eleValue = [this.value];
					}
				} 
				else {
					if($frm.find('input[name='+this.name+']').length > 1) {
						eleValue = this.value;
					}
					else {
						eleValue = [this.value];
					}
				}
			}
		}
		else {
			
			// converter date format
			if($(this).attr('data-date-format') !== undefined) {
				eleValue = $(this).cvtDateFormat();
			}
			
			// converter number only (1,234,567 -> 1234567)
			else if( $(this).attr('data-valid-num') !== undefined || $(this).attr('data-valid-num') == 'true' ){
				eleValue = $(this).unComma();
			}
			
			// other
			else {
				//console.log(this.name + ':' + this.value);
				if($(this).attr('data-serialize-type') !== undefined) {
					if($(this).attr('data-serialize-type') == 'int') {
						eleValue = parseInt(this.value);
					}
				}
				else {
					eleValue = this.value;
				}
				
			}
		}

		// push data
		if (o[this.name] !== undefined) {
			if (!o[this.name].push) {
				o[this.name] = [ o[this.name] ];
			}
			o[this.name].push(eleValue || '');
		} 
		else {
			o[this.name] = eleValue || '';
		}
		
	});
	
	/**
	 * ========================================================================================================
	 *  input type : radio
	 * ========================================================================================================
	 */
	$frm.find('input[type=radio]').each(function(){
		
		if (this.name === null || this.name === undefined || this.name === '') {
			return;
		}
		
		var eleValue = null;
		
		// radio check data only
		if($(this).attr('data-check-only') !== undefined && $(this).attr('data-check-only') == 'true') {
			
			var chkValue = $frm.find('input:radio[name='+this.name+']:checked').val();
			
			if($(this).attr('data-array-always') !== undefined && $(this).attr('data-array-always') == 'true') {
				eleValue = [chkValue];
			}
			else {
				eleValue = chkValue;
			}
			
			// push data
			o[this.name] = eleValue || '';
		}
		else {
			if($(this).attr('data-array-always') !== undefined && $(this).attr('data-array-always') == 'true') {
				
				if($frm.find('input[name='+this.name+']').length > 1) {
					eleValue = this.value;
				} else {
					eleValue = [this.value];
				}
			}
			else {
				eleValue = this.value;
			}
			
			// push data
			if (o[this.name] !== undefined) {
				if (!o[this.name].push) {
					o[this.name] = [ o[this.name] ];
				}
				o[this.name].push(eleValue || '');
			} 
			else {
				o[this.name] = eleValue || '';
			}
		}
		
	});
	
	return o;
};


/**
 * ==================================================================================
 * 폼 오브젝트 데이터 생성(수동)
 * ----------------------------------------------------------------------------------
 * obj_name_list : 데이터 생성에 사용될 object 명 [배열형태]
 * ==================================================================================
 */
$.fn.serializeManualObject = function(obj_name_list) {
		
	var $frm = $(this);
	var obj_cnt = $frm.find('[name='+obj_name_list[0]+']').length;
	
	// 동일 명칭 object가 있을때는 배열로 생성
	if(obj_cnt > 1) {
		
		var rtn_array = new Array();
		for(var i = 0; i < obj_cnt; i++) {
			
			var obj = {};
			for(var j = 0; j < obj_name_list.length; j++) {
				var push_obj = $frm.find('[name='+obj_name_list[j]+']').eq(i);
				obj[obj_name_list[j]] = $(push_obj).val() || '';
			}
			rtn_array[i] = obj;
		}
		
		return rtn_array;
	}
	else {
		var rtn_obj = {};
		for(var i = 0; i < obj_name_list.length; i++) {
			$frm.find('[name='+obj_name_list[i]+']').each(function(){
				rtn_obj[this.name] = $(this).val() || '';
			});
		}
		return rtn_obj;
	}
};

