<template>
  <div id="app" :class="{'login-background':!isLoggedIn}">
    <div v-if="!isLoggedIn">
      <div class="row">
        <div class="login-card">
          <login :dorms="dorms" @login="selectedDorm => login(selectedDorm)"/>
        </div>
      </div>
    </div>
    <div v-else>
      <nav class="navbar navbar-theme">
        <div class="container-theme">
          <ul class="nav navbar-nav">
            <li class="dropdown">
              <a href="" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false" id="dormName">{{ dorm }} <span class="caret"></span></a>
              <ul class="dropdown-menu">
                <li v-for="aDorm in dorms"><a @click="dorm=aDorm">{{ aDorm }}</a></li>
              </ul>
            </li>
            <li class="has-hover-bottom-bar" :class="{active: mode==='current'}"><a @click="mode='current'">현재 수리 요청 목록 <span class="notification">{{newCount(dorm)}}</span> </a></li>
            <li class="has-hover-bottom-bar" :class="{active: mode==='history'}"><a @click="mode='history'">모든 수리 요청 기록</a></li>
          </ul>
          <ul class="nav navbar-nav navbar-right">
          </ul>
        </div>
      </nav>
      <div class="container-theme">
        <div :is="mode" :all-list="allList" :dorm="dorm" :sagam="sagam"></div>
      </div>
    </div>
  </div>
</template>

<script>
import current from './current.vue'
import history from './history.vue'
import login from './login.vue'
import { db } from './firebase'

export default {
  name: 'app',
  data () {
    return {
      isLoggedIn: false,
      dorm: '아름관',
      dorms: [
        '아름관',
        '성실관',
        '진리관',
        '희망관'
      ],
      mode: 'current',
      allList: [],
      sagam: ''
    }
  },
  firebase: {
    allList: {
      source: db.ref('request')
    }
  },
  methods: {
    newCount: function (dormName) {
      if (this.allList.length > 0) {
        return this.allList.filter(x => x.dorm === dormName && x.status === 'new').length
      }
    },
    login (sagam) {
      this.dorm = sagam.slice(0, 3)
      this.sagam = sagam.slice(4)
      this.isLoggedIn = true
    }
  },
  components: {
    current,
    history,
    login
  }
}
</script>

<style>
* {
  text-shadow: none !important;
}

.container-theme {
  max-width: 85em;
  margin: 0 auto;
}

nav.navbar.navbar-theme {
  border-top: 0;
  border-radius: 0;
  border: 0;
  background: whitesmoke;
}

nav.navbar.navbar-theme .navbar-nav a {
  color: #555;
}

nav.navbar.navbar-theme .navbar-nav a:hover,
nav.navbar.navbar-theme .navbar-nav a:focus,
nav.navbar.navbar-theme .navbar-nav a:active {
  background-color: whitesmoke;
}

nav.navbar.navbar-theme .navbar-nav li.active {
  box-sizing: border-box;
  border-bottom: 2px solid #34515e;
}

.has-hover-bottom-bar {
  cursor: pointer;
}

.has-hover-bottom-bar:hover {
  box-sizing: border-box;
  border-bottom: 2px solid #333;
  transition: all 0.3s ease;
}


.title {
  font-size: 22px;
  letter-spacing: 1px;
  margin: 3px 0;
  padding-left: 10px;
  background-color: #34515e;
  color: #FFFFFF;
}

.notification {
  position: absolute;
  top: 5px;
  right: -4px;
  padding: 0 6px;
  border-radius: 100px;
  background-color: #C62828;
  color: white;
  z-index: 2;
}

.reqitem {
  margin:5px;
  padding:5px;
  background-color:#d8d8d8;
  font-size:20px;
}

body {
  font-family: 'Nanum Barun Gothic', sans-serif;
}

table.table tbody tr td {
  vertical-align: middle;
  text-align: left;
}

.list-enter-active {
  transition: all 0.3s;
}

.list-enter {
  opacity: 0.4;
  transform: translateY(3px);
}

#dormName {
  font-size: 20px;
  letter-spacing: 2px;
  padding-left: 20px;
  margin-right: 30px;
  background-color: #34515e;
  color: #FFFFFF;
  border-bottom: 2px solid #34515e;
  text-shadow: none;
}

#newRqButton {
  background-color: #34515e;
  color: #FFFFFF;
}

.top-action-button {
  float: right;
  margin-bottom: 10px;
}

.login-card {
  max-width: 30vw;
  margin: 30vh auto 0;
  padding: 2em;
  border-radius: 2px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
  background-color: white;
}

.login-background {
  background-color: #e5e5e5;
  height: 100vh;
}

</style>
