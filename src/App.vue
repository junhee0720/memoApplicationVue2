<!-- src/App.vue -->
<template>
  <div id="app">
    <!-- 등록한 컴포넌트를 Template 영역에 커스텀 엘리먼트로 추가한다. -->
    <app-header />
  </div>
</template>

<!-- src/App.vue -->
<script>
// 1. 우리가 필요한 컴포넌트를 가져온다.
import AppHeader from './components/AppHeader';


export default {
  name: 'app' ,
  components : {
    // 2. 1에서 가져온 컴포넌트를 등록해준다.
    AppHeader

  }
}
</script>

<style>
/* 앞에서 작성한 reset.css 파일을 가져온다. */
@import "./styles/reset.css";

#app{
  width: 506px;
  margin: 0 auto;
}

</style>
