<template>
    <div>
        <form v-if="!isReg">
            <div class="header">欢迎登录</div>
            <div class="form">
                <label>用户名</label>
                <input type="text" v-model="name">
            </div>
            <div class="form">
                <label>密码</label>
                <input type="password"v-model="password">
            </div>
            <div class="button">
                <div class="first" @click="login()">登录</div>
                <div class="second" @click="reg()">注册</div>
            </div>

        </form>
        <form v-else>
            <div class="header">注册信息</div>
            <div class="form">
                <label>用户名</label>
                <input type="text" v-model="name">
            </div>
            <div class="form">
                <label>密码</label>
                <input type="password"v-model="password">
            </div>
            <div class="form">
                <label>确认密码</label>
                <input type="password" v-model="repeat">
            </div>
            <div class="button">
                <button class="first"  @click="addUser()">確定</button>
                <button class="second"  @click="cancel()">取消</button>
            </div>
        </form>
    </div>
</template>

<script>
    export default {
        name: "Login",
        data() {
            return{
                isReg:false,
                name: '',
                password:'',
                repeat:''
            }

        },
        methods:{
            login() {
                if (localStorage.getItem("name") === this.name && localStorage.getItem("password") === this.password) {
                    this.name = '';
                    this.password = '';
                    this.$router.push('/home/list');
                } else {
                    alert("用戶名或密碼不正確")
                }
            },
            reg() {
                this.isReg = true
            },
            cancel() {
                this.isReg = false
            },
            addUser() {
                //判斷兩次輸出的密碼是否一致
                if (this.password === this.repeat) {
                    localStorage.setItem("name", this.name)
                    localStorage.setItem("password", this.password)
                    this.name = ''
                    this.password = ''
                    this.isReg = false
                }else {
                    alert('兩次密碼不一致')
                }
            }
        }
    }
</script>

<style scoped>
.header{
    margin: 25px;
    font-weight: bold;
    font-size: 20px;
}
.form {
    display: flex;
    justify-content: flex-end;
    margin: 10px;
}
label{
    height: 30px;
    line-height: 20px;
    margin-right: 5px;
}
input {
    width: 75%;
    height: 30px;
    line-height: 30px;
    border-radius: 7px;
    border: 1px solid #d5d5d5;
    padding: 0 10px;
    box-sizing: border-box;
    outline: none;
    -webkit-tap-highlight-color: rgba(0,0,0,0);
}
.button {
    display: flex;
    margin: 30px 20px;
}
button {
    flex: 1;
    margin: 5px;
    height: 35px;
    line-height: 35;
    border-radius: 7px;
    border: 1px solid #42b983;
    cursor: pointer;


}

.first {
    line-height: 2;
     background: #42b983;
     color: #fff;
    flex: 1;
    margin: 5px;
    height: 35px;
    border-radius: 7px;
    border: 1px solid #42b983;
    cursor: pointer;
    text-align: center;
 }
.second{
    line-height:2;
     color: #42b983;
    flex: 1;
    margin: 5px;
    height: 35px;
    border-radius: 7px;
    border: 1px solid #42b983;
    cursor: pointer;
    text-align: center;
 }
</style>
