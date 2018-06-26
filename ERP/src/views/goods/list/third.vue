<template>
    <section>
        <!-- 列表内容 -->
        <div id='k'  class="conent" :style="{height: $store.state.home.modelContentHeight + 'px'}">
            <div style="border: 1px solid #dcdfe6;background:white;overflow:hidden" :style="{height: $store.state.home.modelContentHeight-20 + 'px'}">
                <div>
                    <!-- 高级搜索表单内容 -->
                    <el-card  v-show="morefind" class="box_card" >
                        <div slot="header" class="clearfix">
                            <span :style="{fontSize: '16px'}">高级搜索</span>
                            <i @click="morefind = false" class="el-icon-close" style="float: right; padding: 3px 0; cursor: pointer"></i>
                        </div>
                        <el-form class="myForm" :inline="true" :model="morefrom" label-position="right" size="small" label-width="80px" ref="morefind" >
                            <el-form-item label="商品名称" prop='title'>
                                <el-input v-model="morefrom.title" placeholder="请输入商品名称" ></el-input>
                            </el-form-item>
                            <el-form-item label="商品编号" prop='itemCode'>
                                <el-input v-model="morefrom.itemCode" placeholder="请输入商品编号" ></el-input>
                            </el-form-item>
                            <el-form-item label="商品条码" prop='barCode'>
                                <el-input v-model="morefrom.barCode" placeholder="请输入商品条码" ></el-input>
                            </el-form-item>
                            <br>
                            <el-form-item label="商品分类" prop='categoryName'>
                                <el-cascader
                                    placeholder="请选择"
                                    v-model="morefrom.categoryName"
                                    :options="categoryNameOptions"
                                    change-on-select
                                    :props='props'
                                    size='small'>
                                </el-cascader>
                            </el-form-item>
                            <el-form-item label="商品品牌" prop='brandName'>
                                <el-select filterable v-model="morefrom.brandName" placeholder="全部">
                                    <el-option
                                        v-for="item in brandNameOptions"
                                        :key="item.id"
                                        :label="item.brandName"
                                        :value="item.id">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="贸易类型" prop='sendWay'>
                                <el-select v-model="morefrom.sendWay" placeholder="全部">
                                    <el-option
                                        v-for="item in sendWayOptions"
                                        :key="item.id"
                                        :label="item.categoryName"
                                        :value="item.id">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <br>
                            <el-form-item label="提成方式" prop='deduct'>
                                <el-select v-model="morefrom.deduct" placeholder="全部">
                                    <el-option
                                        v-for="item in deductOptions"
                                        :key="item.promoteGroupId"
                                        :label="item.promoteGroupName"
                                        :value="item.promoteGroupId">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <br>
                            <el-form-item>
                                <el-button @click="findtrue" style="padding:9px 15px;width: 90px" type="primary" @keyup.enter='findtrue'>确定</el-button>
                                <el-button @click="findreturn" style="padding:9px 15px;width: 90px">取消</el-button>
                                <el-button type="text" style="padding:9px 15px;width: 40px" @click="resultfind">清空</el-button>
                            </el-form-item>
                        </el-form>
                    </el-card>
                </div>
                <!-- 列表搜索框 -->
                <div>
                    <ul class="conent_list">
                        <li class="input">
                            <el-input
                                size="small"
                                placeholder="请输入商品名称/条码/ID/编号等"
                                prefix-icon="el-icon-search"
                                :style="{width: '378px'}"
                                v-model="serchText"
                                clearable
                                @keyup.enter.native='goodfind'>
                            </el-input>
                        </li>
                        <li class="find">
                            <el-button  type="primary" style='padding:9px 15px; margin-left:10px' size='small' @click="goodfind">搜索</el-button>
                        </li>
                        <!-- 高级搜索表单 -->
                        <li class="morefind">
                            <el-button type="text" @click="morefind = true">高级搜索</el-button>
                        </li>
                    </ul>
                </div>
                <!-- 列表顶部隐藏对话框   有问题需要改动/消失隐藏没有判断 -->
                <ul class="conent_hidden" v-show="hidden_show">
                    <li @click='isHidden'><i class="el-icon-close"></i></li>
                    <li >已选择 <span style="color:#3f9fff" v-text="selectTableData.length">1</span> 项</li>
                    <li>
                        <el-button type="text" @click="open" v-perss="'报价设置'">停止报价</el-button>
                    </li>
                    <li>
                        <el-button type="text" @click="dialogVisible1 = true" v-perss="'供应商选择框'">更改报价供应商</el-button>
                    </li>
                    <li>
                        <el-button type="text" @click="dialogVisible2 = true" v-perss="'商品提成设置'">更改提成方式</el-button>
                    </li>
                    <li>
                        <el-button type="text" @click="opensell" >出售</el-button>
                    </li>
                    <li>
                        <el-button type="text" @click="opennosell" >禁售</el-button>
                    </li>
                </ul>
                <!-- 列表顶部隐藏谈出框 -->
                <el-dialog title="提示" :visible.sync="dialogVisible1" width="25%" :close-on-click-modal='false'>
                    <el-form label-width="100px">
                        <el-form-item label="供应商分组" size='small'>
                            <el-select v-model="sellerfind" placeholder="选择允许报价的供应商分组"   style="margin-left:20px">
                                <el-option
                                    v-for="item in sellerfindoptions"
                                    :key="item.id"
                                    :label="item.sellerCompanyName"
                                    :value="item.id">
                                </el-option>
                            </el-select>
                        </el-form-item>
                    </el-form>
                    <span slot="footer" class="dialog-footer">
                        <el-button @click="dialogVisible1 = false" size='small'>取 消</el-button>
                        <el-button type="primary" @click="batchconfim" size='small'>确 定</el-button>
                    </span>
                </el-dialog>
                <el-dialog title="提示" :visible.sync="dialogVisible2" width="25%" :close-on-click-modal='false'>
                    <el-form  label-width="100px">
                        <el-form-item label="提成方式" size='small' >
                            <el-select v-model="deduct" placeholder="选择提成方式"  style="margin-left:20px">
                                <el-option
                                    v-for="item in deductOptions"
                                    :key="item.promoteGroupId"
                                    :label="item.promoteGroupName"
                                    :value="item.promoteGroupId">
                                </el-option>
                            </el-select>
                        </el-form-item>
                    </el-form>
                    <span slot="footer" class="dialog-footer">
                        <el-button @click="dialogVisible2 = false" size='small'>取 消</el-button>
                        <el-button type="primary" @click="dialogVisible2 = false" size='small'>确 定</el-button>
                    </span>
                </el-dialog>
                <!-- 商品列表内容 -->
                <div class="center">
                    <el-table
                        :data="goodData"
                        :height="$store.state.home.modelContentHeight-80"
                        ref="goodTable"
                        @selection-change="handleSelectionChange">
                        <!-- check -->
                        <el-table-column
                            type="selection"
                            width="30">
                        </el-table-column>
                        <!-- 商品图片 -->
                        <el-table-column
                            prop="mainImg"
                            label="商品">
                            <template slot-scope="scope" >
                                <!-- <div style="width:60px;height:60px;background: black;"></div> -->
                                <div class="goodimgbox">
                                    <img :src="scope.row.mainImg" alt="">
                                </div>
                            </template>
                        </el-table-column>
                        <!-- 商品内容 -->
                        <el-table-column
                            prop='title'
                            width="300">
                            <template slot-scope="scope">
                                <div @click="openGoods(scope.row.id)" class="goodmore">{{ scope.row.title }}</div>
                            </template>
                        </el-table-column>
                        <!-- 条码 -->
                        <el-table-column
                            prop="barCode"
                            label="条码">
                        </el-table-column>
                        <!-- 编号 -->
                        <el-table-column
                            prop="itemCode"
                            label="编号">
                        </el-table-column>
                        <!-- 规格 -->
                        <el-table-column
                            label="规格-SKU">
                            <template slot-scope="scope">
                                <el-button type="text" @click="lookgoodsku(scope)" v-perss="'sku属性'"><span style="font-size:12px">查看</span></el-button>
                                <el-dialog title="规格明细" :visible.sync="dialogTableVisible" >
                                    <el-table :data="gridData" class='tablebody' >
                                        <el-table-column property="skuPropertyNames" label="规格" align='center'></el-table-column>
                                        <el-table-column property="id" label="SKU编码"  align='center'></el-table-column>
                                    </el-table>
                                </el-dialog>
                            </template>
                        </el-table-column>
                        <!-- 分类 -->
                        <el-table-column
                            prop="categoryName"
                            label="分类">
                        </el-table-column>
                        <!-- 品牌 -->
                        <el-table-column
                            prop="brandName"
                            label="品牌">
                        </el-table-column>
                        <!-- 贸易类型 -->
                        <el-table-column
                            prop="sendWayName"
                            label="贸易类型">
                        </el-table-column>
                        <!-- 提成方式 -->
                        <el-table-column
                            prop="promoteGroupName"
                            label="提成方式">
                        </el-table-column>
                        <!-- 状态 -->
                        <el-table-column
                            prop="onShelf"
                            label="出售状态">
                        </el-table-column>
                        <!-- 操作 -->
                        <el-table-column
                            label="操作"
                            width='200'>
                            <template slot-scope="scope">
                                <el-button type="text" @click="editgoods(scope.row.id)"><span style="font-size:12px" v-perss="'编辑商品基本信息'">编辑</span></el-button>
                                <el-button type="text" @click="openGoods(scope.row.id)"><span style="font-size:12px" >详情</span></el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                    <!-- 查看弹出框 -->
                </div>
            </div>
        </div>

        <!-- 商品分页 -->
        <footer class="bottom" >
            <div class="block" style="margin-top:-14px">
                <el-pagination
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page="currentPage"
                    :page-sizes="[10, 30, 50, 100]"
                    :page-size="10"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="totalPage">
                </el-pagination>
            </div>
        </footer>
    </section>
</template>

<script>
import api from 'api/goods'

export default {
    name : 'third',

    data(){
        return {
            // 搜索框内容
            serchText: '',
            // 隐藏商品顶部弹出框
            hidden_show:false,

            // 商品列表全选及数量
            checkAll: false,
            checkedCities: [],
            isIndeterminate: true,

            // 弹出框状态  查看规格/高级搜索表单/更改供应商/更改提成方式
            dialogTableVisible: false,
            morefind: false,
            dialogVisible1: false,
            dialogVisible2: false,

            // 高级搜索表单内容
            morefrom: {
                pageNo: 1,
                pageSize: 10,
                title : '',
                itemCode : '',
                categoryName : [],
                brandName: [],
                barCode: '',
                sendWay: [],
                deduct: []
            },
            // 选择框隐藏表单内容
            // 更改供应商
            sellerfind:[],
            sellerfindoptions:[],
            // 提成方式
            deductOptions: [],

            // 更改提成方式
            deduct: [],

            // 商品名称分类
            categoryNameOptions: [],
            // 商品品牌名称
            brandNameOptions: [],
            // 贸易类型
            sendWayOptions: [
                {
                    id:'1',
                    categoryName: '国内贸易'
                },
                {
                    id:'2',
                    categoryName: '保税区直供'
                },
                {
                    id:'3',
                    categoryName: '海外直邮'
                }
            ],

            // 查看规格弹出框内容
            gridData: [],
            // 底部分页数
            currentPage: 2,
            totalPage: 1,

            // 商品列表内容
            goodData: [],
            selectTableData: [],

            page: {
                pageNo: 1,
                pageSize: 10,
                isPresent: 1,
                searchWord:''
            },
            props: {
                label:'categoryName',
                value:'id',
                children: 'itemCategories'
            }
        }
    },
    methods: {
        isHidden() {
            this.hidden_show = false;
            this.$refs.goodTable.clearSelection()
        },
        // 停止报价弹出框
        open() {
            this.$confirm('此操作将会停止商品报价, 是否继续?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                let arr = []

                for (let i in this.checkgood){
                    arr.push(this.checkgood[i].id)
                }
                let obj = {
                    ids:arr.toString(),
                    isBidding: 0
                }

                api.putitembatchUpdate(obj).then((res) => {
                    this.$message({
                        type: 'success',
                        message: '已停止报价!'
                    })
                })
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消操作'
                });
            });
        },
        // 出售
        opensell() {
            this.$confirm('此操作将会设置出售状态为出售, 是否继续?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                let arr = []

                for (let i in this.checkgood){
                    arr.push(this.checkgood[i].id)
                }
                let obj = {
                    ids:arr.toString(),
                    onShelf: 1
                }

                api.putitembatchUpdate(obj).then((res) => {
                    this.$message({
                        type: 'success',
                        message: '已更改状态为出售!'
                    })
                })


            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消操作'
                });
            });
        },
        // 禁售
        opennosell() {
            this.$confirm('此操作将会设置出售状态为禁售, 是否继续?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                let arr = []

                for (let i in this.checkgood){
                    arr.push(this.checkgood[i].id)
                }
                let obj = {
                    ids:arr.toString(),
                    onShelf: 0
                }

                api.putitembatchUpdate(obj).then((res) => {
                    this.$message({
                        type: 'success',
                        message: '已更改状态为禁售!'
                    })
                })


            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消操作'
                });
            });
        },
        // 更改供应商
        batchconfim() {
            let arr = []

            for (let i in this.checkgood){
                arr.push(this.checkgood[i].id)
            }
            let obj = {
                ids:arr.toString(),
                supplierAccess: this.sellerfind.toString()
            }

            api.putitembatchUpdate(obj).then((res) => {
                this.dialogVisible1 = false
                this.$message({
                    type: 'success',
                    message: '已更改供应商!'
                })
            })
        },
        // 底部分页函数
        handleSizeChange(val) {
            this.page.pageSize = val
            this.getall()
        },
        handleCurrentChange(val) {
            this.page.pageNo = val
            this.getall()
        },
        // 判断选择框
        handleSelectionChange(val){
            this.selectTableData = val
            if (this.selectTableData.length > 0){
                this.hidden_show = true
            } else {
                this.hidden_show = false
            }
        },
        // 查看规格
        lookgoodsku(scope) {
            this.dialogTableVisible = true
            // console.log(scope.row.id)
            let obj = {
                itemId: scope.row.id
            }

            api.getitemskuProperty(obj).then((response)=>{
                this.gridData = ''
                // console.log(response)
                this.gridData = response.data.list
            }).catch((error)=>{
                console.log(error)
            })
        },
        // 打开详情
        openGoods(data) {
            this.$store.commit('setCurrentModelId', data)
            this.$router.push('goodsDetails')
        },
        // 编辑商品
        editgoods(data) {
            this.$store.commit('setCurrentModelId', data)
            this.$router.push({
                path: '/editGoods/' + '2'
            })
        },
        // 商品搜索
        goodfind() {
            this.page.searchWord = this.serchText
            if (this.serchText == ''){
                this.page = {
                    pageSize: 10,
                    pageNo: 1,
                    itemType: 0,
                    isPresent: 1
                }
            }
            api.getitemlist(this.page).then((response) => {
                // console.log(response.data)
                this.goodData = response.data.list
                this.totalPage = response.data.total
            }).catch((error)=>{
                console.log(error)
            })
        },
        // 确定高级搜索
        findtrue() {
            let obj = {
                pageNo: 1,
                pageSize: 10,
                title : this.morefrom.title,
                itemCode : this.morefrom.itemCode,
                categoryId :this.morefrom.categoryName[this.morefrom.categoryName.length - 1],
                brandId: this.morefrom.brandName.toString(),
                barCode: this.morefrom.barCode,
                sendWay: this.morefrom.sendWay.toString(),
                deduct: this.morefrom.deduct.toString()
            }

            // console.log(obj)
            api.getitemlist(obj).then((response) => {
                // console.log(response.data.list)

                this.goodData = response.data.list
                this.totalPage = response.data.total
                this.morefind = false
                this.$refs['morefind'].resetFields();


            }).catch((error)=>{
                console.log(error)
            })
        },
        findreturn() {
            this.morefind = false
            this.$refs['morefind'].resetFields();
        },
        resultfind() {
            this.$refs['morefind'].resetFields();
        },
        get() {
            api.getitemlist(this.page).then((response) => {
                // console.log(response.data)
                this.goodData = response.data.list
                this.totalPage = response.data.total
                for (let i in this.goodData){
                    if (this.goodData[i].onShelf == 1){
                        this.goodData[i].onShelf = '出售'
                    } else {
                        this.goodData[i].onShelf = '禁售'
                    }
                }

            }).catch((error)=>{
                console.log(error)
            })
        },
        // 查询商品分类
        getcategoryName() {
            api.getcategorylist().then((response)=>{
                // console.log(response.data.list)
                this.categoryNameOptions = response.data

            }).catch((error)=>{
                console.log(error)
            })
        },
        // 查询商品品牌
        getbrandName() {
            let obj = {
                pageNo: 1,
                pageSize: 999999
            }

            api.getitemBrandlist(obj).then((response)=>{
                this.brandNameOptions = response.data.list
            }).catch((error)=>{
                console.log(error)
            })
        },
        // 查询提成方式
        getdeduct() {
            api.getitempromotes().then((response)=>{
                this.deductOptions = response.data.list
            }).catch((error)=>{
                console.log(error)
            })
        },
        // 获取供应商列表
        getsellerfind() {
            api.getsellerfindAll().then((response)=>{
                // console.log(response)
                this.sellerfindoptions = response.data.list
            }).catch((error)=>{
                console.log(error)
            })
        },

        getall() {
            this.get()
            this.getcategoryName()
            this.getbrandName()
            this.getdeduct()
            this.getsellerfind()
        }
    },
    created() {
        this.getall()
    },
    activated() {
        this.getall()
    }
}
</script>
<style scoped>
*{
   list-style: none;
}
#k {
    padding: 10px;
    background: #f5f5f5;
    position: relative;
}
/* 顶部样式 */
 .conent_list{
    display: flex;
    padding: 15px;
    background: white;
    margin: 0;
    height: 60px;
}
 .conent_hidden{
    display: flex;
    background: white;
    margin: 0;
    height: 60px;
    position: absolute;
    left: 11px;
    top:11px;
    z-index: 10;
    width: 98%;
    line-height: 60px;
    padding: 0;
}
.conent_hidden>li{
    margin-left: 30px
}
.morefind{
    margin-left: 20px;
    height: 32px;
    line-height: 32px;
}
.morefind>button{
    padding: 0
}
/* 商品内容样式 */
.goodmore:hover{
    cursor: pointer;
    text-decoration: underline;
}
.center .el-checkbox{
    display: block;
    margin: 0
}
.display{
    display: flex;
    justify-content: space-around;
}
.display>li{
    width: 150px;
}
.text_color{
    color: #409EFF
}
.goods_conent{
   border-top:1px solid #e4e7ed;
   height: 70px;
   padding: 5px 0;
   margin: 0
}
.goods_list{
    background: #f5f5f5;
    height: 40px;
    line-height: 40px;
    margin: 0
}
/* 商品弹出框样式 */

.navdiv>.el-tabs .el-tabs__nav-wrap{
    margin-left: 10px
}
.text_color>.el-button{
    padding: 0
}
.small_pagination{
    margin-left: 700px;
    margin-top:30px;
}
.bottom{
    border-top:1px solid #e5e8e8;
    position: relative;
    /* margin-top: -23px; */
    background: white;
    padding: 27px 30px;
}
.bottom .block{
    position: absolute;
    right: 30px;
    top:12px
}

.el-form input{
    width: 240px
}

.el-icon-close:hover{
    font-size: 14px
}
.goodimgbox img{
    height: 60px;
    width: 60px
}

.box_card{
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    height: 300px;
    z-index: 9;
    margin: 10px;

}
.model_content_inner{
    position: relative;
}
.el-date-editor--daterange.el-input, .el-date-editor--daterange.el-input__inner, .el-date-editor--timerange.el-input, .el-date-editor--timerange.el-input__inner{
    width: 390px;
}
.el-form{
    color: #636365;
}
</style>

