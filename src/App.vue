<template>
  <router-view></router-view>
</template>

<style>
*, *::after, *::before {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
  font-weight: 700;
  stroke: #00684a;
  transition: 200ms;
}
*::-webkit-scrollbar {
  width: 5px;
  border-radius: 20px;
  background-color: #c5c5c5;
}
*::-webkit-scrollbar-thumb {
  border-radius: 20px;
  background-color: #ffffff;
}
*::-webkit-scrollbar-track {
  box-shadow: 0 7px 25px rgba(#00684a, .8);
  background-color: #c5c5c5;
}
html, body {
  height: 100%;
}
html {
  font-size: 16px;
}
body {
  overflow-y: auto;
  min-height: 100vh;
  background-color: #ffffff;
  color: #001e2b;
}
ul, ol, li {
  list-style: none;
}
a {
  text-decoration: none;
  color: inherit;
}
.container {
  position: relative;
  width: 100%;
}
.title {
  font-size: 1.5625rem;
  line-height: 1.5625rem;
}
@media screen and (max-width: 850px) {
  html {
    font-size: 13px;
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: 200ms;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateX(-25px);
}
</style>