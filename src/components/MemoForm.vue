<!-- src/components/MemoForm.vue -->
<template>
	<div class="memo-form">
		<!-- 1. form을 이용하여 사용자가 입력할 수 있는 필드를 추가한다.
			form의 submit 이벤트가 발생하면 이벤트의 기본동작을 방지된 후,
			우리가 아래에 선언한 addMemo 함수가 콜백 함수로 실행된다.
		 -->
		<form @submit.prevent="addMemo">
			<fieldset>
				<div>
					<input class="memo-form__title-form"
							type="text" 
							v-model="title"
							placeholder="메모의 제목을 입력해주세요." />
					<textarea class="memo-form__content-form"
					v-model="content"
					placeholder="메모의 내용을 입력해주세요." />
					<!-- 앞에서 추가해준 폰트어썸은 다음과 같이 클래스를 이용하여 아이콘을 사용할 수 있다. -->
					<button type="reset"><i class="fas fa-sync-alt"></i> </button>
				</div>
				<button type="submit">등록하기</button>
			</fieldset>
		</form>
	</div>
</template>

<script>
	export default {
		name: "MemoForm" ,
		data () {
			return {
				title : '',
				content: '',
			};
		},
		methods: {
			resetFields() {
				this.title = '';
				this.content = '';	
			},
			addMemo(){
				// 비구조화 할당(destructuring assignment) 구문 을 이용하여 변수를 선언한다.
				const { title, content } = this;
				// 데이터의 고유한 식별자를 생성한다.
				const id = new Date().getTime();

				// 제목이나 내용을 입력하지 않은 경우를 대비하여 방어 코드를 추가한다.
				const isEmpty = title.length <= 0 || content.length <= 0;
				if(isEmpty) {
					return false;
				}

				// addMemo 이벤트를 발생시키고 payload로 사용자가 입력한 데이터를 넣어준다.
				this.$emit('addMemo', {id, title, content});

				// 부모 컴포넌트에 데이터를 전파한 후 데이터를 다시 원상태로 초기화한다.
				this.resetFields();
			},
		}
	}
</script>
<!-- scoped 옵션을 사용해 해당 컴포넌트에서만 스타일이 적용될 수 있도록 하였다. -->
<style scoped>
	.memo-form{
		margin-bottom: 24px;
		padding-bottom: 40px;
		border-bottom: 1px solid #eee;
	}
	.memo-form form fieldset div {
		position: relative;
		padding: 24px;
		margin-bottom: 20px;
		box-shadow: 0 4px 10px -4px rgba(0, 0, 0, 0.2);
		background-color: #FFFFFF;
	}
	.memo-form form fieldset div button[type="reset"] {
		position: absolute;
		right: 20px;
		bottom: 20px;
		font-size: 16px;
		background: none;
	}
	.memo-form form fieldset .memo-form__title-form {
		width: 100%;
		margin-bottom: 12px;
		font-size: 18px;
		line-height: 26px;
	}
	.memo-form form fieldset .memo-form__content-form {
		width: 100%;
		height: 66px;
		font-size: 14px;
		line-height: 22px;
		vertical-align: top;
	}
	.memo-form input:focus {
		outline: none;
	}
</style>