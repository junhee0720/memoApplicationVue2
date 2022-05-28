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
						@updateMemo="updateMemo"
			 />
		</ul>
	</div>
</template>

<script>
// 1. axios 라이브러리를 추가한다.
import axios from 'axios';

import Memo from './Memo.vue';
import MemoForm from './MemoForm.vue';

const memoAPICore = axios.create({
	baseURL : 'http://localhost:8000/api/memos'
});


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
		// 기존 로컬스토리지를 사용하는 코드를 삭제한다.
		// this.memos = localStorage.memos ? JSON.parse(localStorage.memos) : [];
		// 앞서 생성된 Axios객체의 get 메소드를 이용하여 데이터를 받아온다.
		memoAPICore.get('/')
			.then(res => {
				// 3. 받아온 데이터를 data의 memos에 저장한다.
				this.memos = res.data;
			})
	},
	methods : {
		addMemo(payload) {
			// MemoForm에서 올려 받은 데이터를 먼저 컴포넌트 내부 데이터에 추가한다.
			//this.memos.push(payload);
			//this.storeMemo();

			// 1. axois 객체의 post 메소드를 이용하여 데이터를 추가한다.
			memoAPICore.post('/',payload)
				.then(res => {
					// 2. 정상적인 메모를 생성 후, 결과값을 memos에 추가한다.
					this.memos.push(res.data);
				});
		},
		storeMemo () {
			const memosToString = JSON.stringify(this.memos);
			localStorage.setItem('memos', memosToString);
		},
		deleteMemo (id) {
			// 1. 자식 컴포넌트에서 인자로 전달해주는 id에 해당하는 메모 데이터의 인덱스를 찾는다.
			const targetIndex = this.memos.findIndex( v => v.id === id);
			// 2. 찾은 인덱스 번호에 해당하는 메모 데이터를 삭제한다.
			// this.memos.splice(targetIndex, 1);
			// 3. 삭제된 후의 데이터를 다시 로컬스토리지에 마찬가지로 저장한다.
			// this.storeMemo();
			// 1. 삭제 대상과 일치하는 id 값을 delete 메소드와 함께 요청한다.
			memoAPICore.delete(`/${id}`)
				.then(() => {
					// 2. 요청 후, MemoApp 컴포넌트의 memos 데이터에서도 삭제한다.
					this.memos.splice(targetIndex, 1);
				});
		},
		updateMemo(payload) {
			// 1. 수정된 메모를 저장한다.
			const {id, content} = payload;
			console.table("payload = "+payload);
			const targetIndex = this.memos.findIndex(v => v.id === id);
			const targetMemo = this.memos[targetIndex];
			// 1. 수정 대상과 일치하는 id값과 수정된 단락에 대한 데이터를 delete put 과 함께 요청한다.

			memoAPICore.put(`/${id}`, {content})
				.then(() => {
					// 2. 요청후 MemoApp 컴포넌트의 memos 데이터에서도 해당하는 데이터를 업데이트한다.
					this.memos.splice(targetIndex, 1, {...targetMemo, content});
			});

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