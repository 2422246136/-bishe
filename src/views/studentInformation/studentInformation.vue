<template>
    <div class="studentInformation">
        <Row class="searchItem">
            <Col span='8' class="item">
                <Col span="4" class="name">
                    班级：
                </Col>
                <Col span="20">
                    <Input v-model="classes" placeholder="请输入班级" class="inputClass" />
                </Col>
            </Col>
            <Col span='8' class="item">
                <Col span="4" class="name">
                    姓名：
                </Col>
                <Col span="20">
                    <Input v-model="name" placeholder="请输入姓名" class="inputClass"/>
                </Col>
            </Col>
            <Col span='8' class="item">
                <Col span="4" class="name">
                    学号：
                </Col>
                <Col span="20">
                    <Input v-model="no" placeholder="请输入学号" class="inputClass"/>
                </Col>
            </Col>
            <Col span='8' class="item">
                <Col span="4" class="name">
                    学年：
                </Col>
                <Col span="20">
                    <Select style="width:75%" v-model="school_year">
                        <Option value="2016-2017">2016-2017</Option>
                        <Option value="2017-2018">2017-2018</Option>
                    </Select>
                </Col>
            </Col>
            <Col span='8' class="item">
                <Col span="4" class="name">
                    学期：
                </Col>
                <Col span="20">
                    <Select style="width:75%" v-model="term">
                        <Option value="第一学期">第一学期</Option>
                        <Option value="第二学期">第一学期</Option>
                    </Select>
                </Col>
            </Col>
            <Col span='8' class="item">
                <Button type="primary" @click="search">查询</Button>
            </Col>
        </Row>
        <div class="tableData">
            <Table border :columns="columns" :data="supplierData"></Table>
        </div>
        <div class="paging">
            <Page :total='total' show-sizer @on-change="onChange" @on-page-size-change="onPageSizeChange" :current="pageNum"
            :page-size="pageSize"/>
        </div>
        <addSupplierModal v-if="showModal" :showModalState='showModal' @changeState='changeState' :currentRowData='currentRowData' />
    </div>
</template>
<script>
import {Button,Table,Page,Input,Select,Option} from 'view-design'
import addSupplierModal from './modal/addSupplierModal'
export default {
    name: 'studentInformation',
    components: {Button,Table,addSupplierModal,Page,Input,Select,Option},
    data(){
        return {
            showModal:false,
            formPage:{
            },
            currentRowData:'',//当前行数据
            columns:[
                {
                    title: '学生姓名',
                    key: 'name'
                },
                {
                    title: '学生学号',
                    key: 'no'
                },
                 {
                    title: '班级',
                    key: 'classes'
                },
                {
                    title: '分数',
                    key: 'fraction'
                },
                {
                    title: '学分',
                    key: 'credit'
                },
                {
                    title: '学年',
                    key: 'school_year'
                },
                {
                    title: '学期',
                    key: 'term'
                },
                {
                    title: '操作',
                    key: 'action',
                    align: 'center',
                    render: (h, params) => {
                        return h('div', [
                            h('span', {
                                props: {},
                                style: {
                                    marginRight: '5px',
                                    display:'inline-block',
                                    width:'70px',
                                    height:'30px',
                                    background:'#2d8cf0',
                                    color:'#fff',
                                    lineHeight:'30px',
                                    borderRadius:'4px',
                                    cursor:'pointer'
                                },
                                on: {
                                    click: () => {
                                        this.addSupplier(params.row)
                                    }
                                }
                            }, '删除成绩'),
                        ]);
                    }
                }
            ],
            supplierData:[
                {
                    name:'1',
                    no:'1',
                    fraction:'1',
                    credit:'1',
                    school_year:'1',
                    term:'1',
                }
            ],
            classes:"",
            no:"",
            name:'',
            school_year:'2016-2017',
            term:'第一学期',
            pageNum:1,
            pageSize:10,
            total:100
        }
    },
    created(){
        // this.list();
    },
    methods: {
        search(){},
        show(val){
            console.log(val)
            this.showModal = true;
            this.currentRowData = val.row;
        },
        addSupplier(item){
            this.showModal = true;
            this.currentRowData = item
        },
        changeState(val){
            this.showModal = val;
            this.list();
        },
        //分页
        onChange (val) {
            this.formPage.pageNum = val;
            this.list();
        },
        //分页大小
        onPageSizeChange (val) {
            this.formPage.pageSize = val;
            this.list();
        },
        //查询列表
        list(){
            this.axios({
                url:'/dcf/supplier/selectAllSupplier',
                method:'get',
                params:this.formPage
            }).then((res)=>{
                if(res.code === 200){
                    console.log(res.data)
                    this.supplierData = res.data.list
                    this.total = res.data.total
                }
            })
        }
    },
    computed: {},
    watch: {}
}
</script>

<style lang="scss" scoped>
    .studentInformation{
        .searchItem{
            background: #fff;
            padding-top: 20px;
            padding-left: 20px;
            .item{
                margin-bottom: 30px;
                .name{
                    vertical-align: bottom;
                    text-align: center;
                    margin-top: 7px;
                }
                .inputClass{
                    width: 75%;
                    vertical-align: top;
                }
            }
        }
        .btn{
            button{
                border-radius: 15px;
            }
        }
        .tableData{
            margin-top: 20px;
        }
        .paging{
            text-align:center;
            margin-top: 10px;
        }
    }
</style>
<style lang='scss'>
    .studentInformation{
        input{
            border: 1px solid #C5D2E4;
            border-radius: 5px;
        }
    }
</style>