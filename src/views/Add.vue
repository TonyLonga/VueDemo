<template>
<div>
    <div class="main">
        <p>标题</p>
        <input class="title" type="text" v-model="title">
        <p>内容</p>
        <textarea class="content"  v-model="content"></textarea>
    </div>
    <div class="button">
    <button  @click="add()">完成</button>
    <button  @click="cancel()">取消</button>
    </div>
</div>
</template>

<script>
    import store from '@/store'
    export default {
        name: "Add",
        store,
        data() {
            return{
                title: '',
                content:''
            }
        },
        methods:{
            //添加内容 add方法
            add() {
                //添加发布时间方法
                var newTime = new Date()
                var year = newTime.getFullYear()
                var month = ('0' + (newTime.getMonth() + 1)).slice(-2)
                var day = ('0' + newTime.getDate()).slice(-2)
                var hours = ('0' + newTime.getHours()).slice(-2)
                var minutes = ('0' + newTime.getMinutes()).slice(-2)
                var addTime = year + '-' + month + '-' + day + ' ' + hours + ':'+ minutes
                store.commit('addItem',{
                    title:this.title,
                    content: this.content,
                    time:addTime,
                    user:sessionStorage.getItem('currentUser')
                })
                //清空内容
                localStorage['pageLists']= JSON.stringify(store.state.lists)//存到本地
                this.title = ''
                this.content = ''
                this.$router.push('/home/list')
            },
            cancel(){
                this.$router.push('/home/user')
            }
        }
    }
</script>

<style scoped>
    .main{
        margin: 10px;
    }
    p{
        margin-bottom: 5px;
    }
    input,textarea {
        border: 1px solid #d5d5d5;
        border-radius: 7px;
        width: 100%;
        box-sizing: border-box;
        font-size: 16px;
        font-family:"Microsoft YaHei UI";
    }
    .title{
        padding: 0 10px;
        height: 30px;
        line-height: 30;
        outline: none;
        -webkit-tap-highlight-color: rgba(0,0,0,0);
    }
    .content{
        height: 350px;
        line-height: 1.6;
        padding: 10px 10px;
        outline: none;
        -webkit-tap-highlight-color: rgba(0,0,0,0);
    }
    .button{
        display: flex;
        margin: 45px 20px;
    }
    button {
        flex: 1;
        margin: 5px;
        height: 35px;
        line-height: 0;
        border-radius: 7px;
        border: 1px solid #42b983;
        cursor: pointer;

    }
    button:nth-child(1) {
        background: #42b983;
        color: #fff;
    }
    button:nth-child(2) {
        background: #42b983;
    }

</style>
