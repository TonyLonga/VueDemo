<template>
    <div>
        <div class="header" v-if="isNone">还没有添加任何便签哦~~</div>
        <div class="item-container" v-if="!isDetail">
            <div class="item" v-for="(item,index) in pageLists" :key="index" @click="toggleItem(index)" :class="{'active' : index === activeIndex}">
                <span class="delete" v-show="index==activeIndex" @click.stop="delItem(index)">&times;</span>
                <p class="title">{{item.title}}<p>
                <p class="content">{{item.content}}</p>
                <div class="footer">
                    <div>{{item.time}}</div>
                    <a v-if="index === activeIndex" @click.stop="goDetail(index)">查看详情>></a>
                </div>
            </div>
        </div>
        <div class="detail" v-else>
            <p class="title">{{detailTitle}}</p>
            <p class="content-t">{{detailContent}}</p>
            <button @click="backList()">返回</button>
        </div>
    </div>
</template>

<script>
    import store from '@/store'
    export default {
        store,
        data () {
            return {
                activeIndex: -1,
                isDetail: false,
                detailTitle: '',
                detailContent: ''
            }
        },
        mounted(){
            store.commit('getLocal')
        },
        computed: {
            pageLists () {
                return store.getters.myLists
            },
            isNone () {
                if (this.pageLists.length === 0) {
                    return true
                } else {
                    return false
                }
            },
        },
        methods: {
            toggleItem (index) {
                if (this.activeIndex === index) {
                    this.activeIndex = -1
                } else {
                    this.activeIndex = index
                }
            },
            delItem (index) {
                store.commit('delItem', index)
                localStorage["pageLists"]=JSON.stringify(store.state.lists) // 存到本地
            },
            goDetail (index) {
                this.isDetail = true
                this.detailTitle = this.pageLists[index].title
                this.detailContent = this.pageLists[index].content
            },
            backList () {
                this.isDetail = false
            }
        },
        // watch: {
        //   pageLists: {
        //     handler: function (newVal) {
        //       localStorage["pageLists"]=JSON.stringify(newVal)
        //     },
        //     deep: true
        //   }
        // } // list组件不更新 更新是在add中完成 或者在app.vue中监听
    }
</script>

<style scoped>
    .header{
    text-align: center;
    margin: 5px;
    font-size:14px
    }
    .item-container{
    position:absolute;
    top:0;
    bottom:60px;
    overflow:auto;
    width:100%
    }
    .item {
        position: relative;
        border: 1px solid #d5d5d5;
        border-radius: 7px;
        margin: 10px;
        font-size: 14px;
        box-shadow: 2px 3px 3px #d5d5d5;
    }
    .title{
        height: 25px;
        line-height: 2;
        padding: 5px;
        font-weight: bold;
        border-bottom: 1px solid #d5d5d5;
    }
    .content {
        height: 25px;
        line-height: 2;
        padding: 5px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
    }
    .footer {
        height: 20px;
        display: flex;
        justify-content: space-between;
        font-size: 12px;
        margin: 5px;
    }
    a{
        text-decoration: underline;
        cursor: pointer;
    }
active{
    color: #ffffff;
    background: #42b983;
}
    .delete {
        position: absolute;
        top: 5px;
        left: 94%;
        font-size: 20px;
        cursor: pointer;
    }
    .delete:hover {
        color: red;
        font-weight: bold;
        font-size: 22px;
    }
    .detail{
        position: absolute;
        overflow: auto;
        width: 100%;
    }
    .title{
        padding: 10px;
        font-weight: bold;
        border-bottom: 1px solid #d5d5d5;
    }
    .content-t {
        font-size: 14px;
        padding: 15px;
        letter-spacing: 2px;
        word-wrap: break-word;
    }
    button {
        width: 90%;
        margin-left: 5%;
        margin-top: 30px;
        margin-bottom: 202px;
        height: 35px;
        line-height: 2;
        border-radius: 7px;
        border: 1px solid #42b983;
        background-color: #42b983;
        color: #ffffff;
        cursor: pointer;
    }
</style>
