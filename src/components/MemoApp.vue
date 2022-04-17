<!-- src/components/MemoApp.vue -->
<template>
	<div class="memo-app">
		<!-- <memo-form v-on:addMemo="addMemo" />과 아래의 코드는 같은 의미이다. -->
		<memo-form @addMemo="addMemo" />
		<ul class="memo-list">
			<memo v-for="memo in memos"
						:key="memo.id"
						:memo="memo"
						@deleteMemo="deleteMemo"
			 />
		</ul>
	</div>
</template>

<script>
import Memo from './Memo.vue';
import MemoForm from './MemoForm.vue';


export default {
  components: { MemoForm, Memo },
	name : 'MemoApp',
	data () {
		return {
			memos: [],
		};
	},
	created () {
		// 1. 만약에 기존에 추가된 localstorage에 데이터가 있다면 created 혹에서
		// localSotage의 데이터를 컴포넌트 내의 memos 데이터에 넣어주고, 
		// 그렇지 않다면 그대로 빈 배열로 초기화한다.
		this.memos = localStorage.memos ? JSON.parse(localStorage.memos) : [];
	},
	methods : {
		addMemo(payload) {
			// MemoForm에서 올려 받은 데이터를 먼저 컴포넌트 내부 데이터에 추가한다.
			this.memos.push(payload);
		},
		storeMemo () {
			const memosToString = JSON.stringify(this.memos);
			localStorage.setItem('memos', memosToString);
		},
		deleteMemo (id) {
			// 1. 자식 컴포넌트에서 인자로 전달해주는 id에 해당하는 메모 데이터의 인덱스를 찾는다.
			const targetIndex = this.memos.findIndex( v => v.id === id);
			// 2. 찾은 인덱스 번호에 해당하는 메모 데이터를 삭제한다.
			this.memos.splice(targetIndex, 1);
			// 3. 삭제된 후의 데이터를 다시 로컬스토리지에 마찬가지로 저장한다.
			this.storeMemo();
		},
		updateMemo(payload) {
			// 1. 수정된 메모를 저장한다.
			const {id, content} = payload;
			const targetIndex = this.memos.findIndex(v => v.id === id);
			const targetMemo = this.memos[targetIndex];
			this.memos.splice(targetIndex, 1, {...targetMemo, content});
			this.storeMemo();
		}
	},
}

</script>
<style scoped>
	.memo-list {
		padding: 20px 0;
		margin: 0;
	}
</style>