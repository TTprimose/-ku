<template>
    <section id='add' >
        <!-- 顶部面包屑 -->
        <header class="add_top">
            <el-breadcrumb separator='-' style="line-height:45px;font-size:15px">
                <el-breadcrumb-item :to="{ path: '/goodsList' }">商品列表</el-breadcrumb-item>
                <el-breadcrumb-item>编辑商品</el-breadcrumb-item>
            </el-breadcrumb>
        </header>
        <!-- 主体内容 -->
        <section class="add_conent" :style="{height: $store.state.home.modelContentHeight + 'px'}">
            <div class="conent_box">
                <!-- 内容 -->
                <el-form style='margin:0'>
                    <!-- 商品title -->
                    <el-form-item class="good_title" >
                        <p class="goodtitle_p">商品类型</p>
                    </el-form-item>
                    <!-- 商品类型 -->
                    <el-form-item class="good_type">
                        <el-button
                            plain
                            @click="option($event)"
                            style="position:relative"
                            class="parent">
                            <div class='goodbutton'>
                                <p>实物类商品</p>
                                <span style='color:#8f9294'>（物流发货）</span>
                                <em class="jiao active"><i class="el-icon-check" style="position:absolute;bottom:-20px;right:0"></i></em>
                            </div>
                        </el-button>
                        <el-button
                            plain
                            @click="option($event)"
                            style="position:relative"
                            class="parent">
                            <div class='goodbutton'>
                                <p>服务类商品</p>
                                <span style='color:#8f9294'>（无需物流）</span>
                                <em class="jiao"><i class="el-icon-check" style="position:absolute;bottom:-20px;right:0"></i></em>
                            </div>
                        </el-button>
                        <el-button
                            plain
                            @click="option($event)"
                            style="position:relative"
                            class="parent">
                            <div class='goodbutton'>
                                <p>赠品</p>
                                <span style='color:#8f9294'>（非售商品）</span>
                                <em class="jiao"><i class="el-icon-check" style="position:absolute;bottom:-20px;right:0"></i></em>
                            </div>
                        </el-button>
                    </el-form-item>
                    <!-- 信息title -->
                    <el-form-item class="good_title">
                        <p class="goodtitle_p">基本信息</p>
                        <el-checkbox class="position" @change="checkboxChangeEvent" v-model="form.expirationDateStatus"><span style="font-size:10px">有无保质期</span></el-checkbox>
                    </el-form-item>
                    <!-- 信息内容 -->
                    <el-form-item class="msg_conent" >
                        <el-form ref="editGoodsForm" :model="form" label-width="100px" :rules="rules">
                            <el-form-item label="商品名称" prop='title'  style="margin-bottom:25px">
                                <el-input v-model="form.title" size="small" placeholder="请输入商品名称" style="width:688px"></el-input>
                            </el-form-item>
                            <el-form-item label="商品卖点" style="margin-bottom:25px">
                                <el-input type="textarea" v-model="form.sellingPoint" size="small" placeholder="请输入商品卖点，长度<300" rows='2'></el-input>
                            </el-form-item>
                            <el-form-item label="购买提醒" style="margin-bottom:25px">
                                <el-input type="textarea" v-model="form.buyRemind" size="small" placeholder="请输入购买提醒，长度<300" rows='2'></el-input>
                            </el-form-item>
                            <el-form-item style="margin-left:-100px;margin-bottom:25px">
                                <div style="display:flex">
                                    <el-form-item label="商品条码" prop='barCode' >
                                        <el-input v-model="form.barCode" size="small" placeholder="请输入商品条码" ></el-input>
                                    </el-form-item>
                                    <el-form-item label="商品编码" prop='itemCode' style="margin-left:70px">
                                        <el-input v-model="form.itemCode" size="small" placeholder="请输入商品编码" ></el-input>
                                    </el-form-item>
                                </div>
                            </el-form-item>
                            <el-form-item style="margin-left:-100px;margin-bottom:25px">
                                <div style="display:flex">
                                    <el-form-item  label="商品品牌" prop='brandId' >
                                        <el-select filterable v-model="form.brandId" placeholder="请选择商品品牌"  size='small'>
                                            <el-option
                                                v-for="item in brandIdoptions"
                                                :key="item.id"
                                                :label="item.brandName"
                                                :value="item.id"></el-option>
                                        </el-select>
                                    </el-form-item>
                                    <el-form-item label="商品分类" prop='categoryId' style="margin-left:70px">
                                        <!-- categoryId返回给数组的时候用这个 -->
                                        <el-cascader
                                            placeholder="请选择商品分类"
                                            v-model="form.categoryId"
                                            :options="categoryIdoptions"
                                            change-on-select
                                            :props='props'
                                            size='small'
                                            style="width:258px">
                                        </el-cascader>
                                        <!-- <el-select v-model="form.categoryId" placeholder="请选择"  size='small'>
                                            <el-option
                                                v-for="item in categoryIdoptions"
                                                :key="item.id"
                                                :label="item.categoryName"
                                                :value="item.id"></el-option>
                                        </el-select> -->
                                    </el-form-item>
                                </div>
                            </el-form-item>
                            <el-form-item style="margin-left:-100px;margin-bottom:25px">
                                <div style="display:flex">
                                    <el-form-item label="贸易类型" prop='sendWay' >
                                        <el-select v-model="form.sendWay" placeholder="请选择贸易类型"  size='small'>
                                            <el-option
                                                v-for="item in sendWayoptions"
                                                :key="item.id"
                                                :label="item.categoryName"
                                                :value="item.id"></el-option>
                                        </el-select>
                                    </el-form-item>
                                    <el-form-item label="建议零售价"  class="money" style="margin-left:70px">
                                        <el-input v-model="form.suggestedPrice" size="small" placeholder="请输入建议零售价" ></el-input>
                                    </el-form-item>
                                </div>
                            </el-form-item>
                            <el-form-item style="margin-left:-100px;margin-bottom:25px">
                                <div style="display:flex">
                                    <el-form-item label="单位" >
                                        <el-select v-model="form.unitId" placeholder="请选择商品单位"  size='small'>
                                            <el-option
                                                v-for="item in unitIdoptions"
                                                :key="item.id"
                                                :label="item.unit"
                                                :value="item.id"></el-option>
                                        </el-select>
                                    </el-form-item>
                                    <el-form-item label="保质期" prop='expirationDateName' :required="requshow" style="margin-left:70px">
                                        <el-input v-model="form.expirationDateName" size="small" >
                                            <template slot="append">单位: ‰</template>
                                        </el-input>
                                    </el-form-item>
                                </div>
                            </el-form-item>
                        </el-form>
                    </el-form-item>
                    <!-- 规格title -->
                    <el-form-item class="good_title">
                        <p class="goodtitle_p">商品规格</p>
                    </el-form-item>
                    <!-- 商品规格 -->
                    <el-form-item class="standard">
                        <el-form label-width="100px">
                            <div>
                                <!-- 商品规格 -->
                                <el-form-item label="商品规格" required v-model="skuProperty">
                                    <div class="add_standard">
                                        <div v-for="(item, index) in skuProperty" :key="index">

                                            <div class="add_small_standard" >
                                                <el-form-item label="规格名" style="margin-left:-20px" >
                                                    <el-select :disabled="!item.showClose" v-model="item.skuPropertyName" @change="changeSku(item, index, skuList)"  placeholder="请选择活动区域" size='small' style="width:138px">
                                                        <el-option
                                                            v-for="item2 in skuList"
                                                            :key="item2.id"
                                                            :label="item2.skuPropertyName"
                                                            :value="item2.skuPropertyName"
                                                            :disabled="item2.disabled">
                                                        </el-option>
                                                    </el-select>
                                                    <!-- <el-checkbox v-model="checked" style="margin-left:10px"  @change="changeupload" v-if='index == 0'>添加规格图片</el-checkbox> -->
                                                    <i v-if="item.showClose"  class="el-icon-circle-close"  style='float:right;margin-top:15px;font-size:16px;cursor:pointer' @click="delskuproperty(index)" ></i>
                                                </el-form-item>
                                            </div>
                                            <el-form-item label='规格值' style="margin-left:-10px;margin-top:5px" >
                                                <span v-for="(itemName, i) in item.itemSkuPropertyValueDTOS" :key='i'>
                                                    <el-select :disabled="!itemName.showClose" @change="changeSKUname(item, index)" placeholder="请选择活动区域" size='small'  v-model="itemName.skuPropertyValueName" style="width:138px;margin-right:20px">
                                                        <el-option
                                                            v-for="item3 in item.itemSkuDTOS"
                                                            :key="item3.id"
                                                            :label="item3.skuPropertyValueName"
                                                            :value="item3.skuPropertyValueName"
                                                            :disabled="item3.disabled">
                                                        </el-option>
                                                    </el-select>
                                                    <i v-if="itemName.showClose" class="el-icon-circle-close"  @click="delskuname(index, i)" style="position:relative;right:33px;top:-15px;font-size:16px;cursor: pointer"></i>
                                                </span>
                                                <el-button type='text' @click="addSpec(index)">添加规格值</el-button>
                                                <div style="display:flex" class="smallupload">
                                                    <div style="width:158px" v-for='(itemImg,index) in item.itemSkuPropertyValueDTOS' :key='index'>
                                                        <div v-if='uploadshow' v-show='index == 0'>
                                                            <el-upload
                                                                :action="upDateImgUrl"
                                                                :data='sentData'
                                                                :file-list='conTinue[index].fileList'
                                                                list-type="picture-card"
                                                                :on-preview="smallPictureCardPreview"
                                                                :on-success='smallAvatarSuccess'
                                                                :on-remove="smallRemove"
                                                                :limit= 1>
                                                                <i class="el-icon-plus"></i>
                                                            </el-upload>
                                                            <!-- action上传地址 -->
                                                            <el-dialog :visible.sync="dialogVisible2">
                                                                <img width="100%" :src="dialogImageUrl2" alt="">
                                                            </el-dialog>
                                                        </div>
                                                    </div>
                                                </div>
                                                <!-- <p style="color:#888;font-size:12px;margin-top:30px">仅支持为第一组规格设置图片（最多40张图），买家选择不同规格会看到对应图片，建议尺寸：640 X 640 像素</p> -->
                                            </el-form-item>
                                        </div>
                                        <!--<el-form-item>-->
                                        <!--<div>-->
                                        <!--<div class="add_small_standard">-->
                                        <!--<el-button type="primary" size='small' @click="addSpecItem">添加规格项目</el-button>-->
                                        <!--</div>-->
                                        <!--</div>-->
                                        <!--</el-form-item>-->
                                    </div>
                                    <p class="standard_p">如有颜色、尺码等多种规格，请添加商品规格</p>
                                </el-form-item>
                                <el-form-item required label="包装类型">
                                    <div class="add_standard">
                                        <div class="add_small_standard add_standard">
                                            <el-form-item  label="类型名" label-width="70px">
                                                <el-select v-model="typesname" :disabled="typesnameFlag" placeholder="选择类型名" size='small'  @change='gettypevalue()'>
                                                    <el-option
                                                        v-for="item in typenameoptions"
                                                        :key="item.id"
                                                        :label="item.unitMsg"
                                                        :value="item.id">
                                                    </el-option>
                                                </el-select>
                                            </el-form-item>
                                        </div>
                                        <div style="padding:10px;display:flex" class="supply_check">
                                            <el-form-item label="类型值" label-width="70px">
                                                <span style="position:relative;margin-right:20px" v-for="(items, index) in typesvalue" :key="index" >
                                                    <el-select :disabled="items.showCloseFlag" v-model="items.id" placeholder="选择类型值" size='small' @change="typechange(index)" >
                                                        <el-option
                                                            v-for="item in typevalueoptions"
                                                            :key="item.id"
                                                            :label="item.supplyMsg"
                                                            :value="item.id"
                                                            :disabled="item.disabled">
                                                        </el-option>
                                                    </el-select>
                                                    <i v-if="!items.showCloseFlag" class="el-icon-circle-close" @click="delet(index)" style="position:relative;right:13px;top:-15px;font-size:16px;cursor: pointer"></i>
                                                </span>
                                            </el-form-item>
                                            <el-button type="text"  style="position: relative;z-index: 20;" @click="add">添加规格值</el-button>
                                        </div>
                                    </div>
                                </el-form-item>

                                <el-form-item label="规格明细">
                                    <!-- 规格明细 -->
                                    <table class='skuTable' style="width: 100%;margin-top:10px">
                                        <tr>
                                            <th width='50%'>
                                                <div class="">
                                                    商品规格
                                                </div>
                                                <div class="tableBox">
                                                    <span :key="index" v-for="(item, index) in skuProperty">{{item.skuPropertyName}}</span>
                                                </div>
                                            </th>
                                            <th width='25%'>SKU编码</th>
                                            <th width='25%'>包装类型</th>
                                        </tr>
                                        <tr  :key="indexTable" v-for="(itemName, indexTable) in showSkuArr">
                                            <td>
                                                <div class="tableBox2">
                                                    <span :key="i" v-for='(skuName, i) in itemName.name'>{{skuName}}</span>
                                                </div>
                                            </td>
                                            <td>
                                                <div class="tableBox2">
                                                    <span v-text="itemName.skuId"></span>
                                                </div>
                                            </td>
                                            <td>
                                                <div :key="k" v-for='(supName, k) in typesvalue'>
                                                    <span >{{supName.supplyMsg}}</span>
                                                </div>
                                            </td>
                                        </tr>
                                    </table>
                                </el-form-item>
                            </div>
                        </el-form>
                    </el-form-item>
                    <!-- 商品属性title -->
                    <el-form-item class="good_title">
                        <p class="goodtitle_p">商品属性</p>
                        <span class="position" style="width:470px">商品属性是对商品的补充说明，如材质、产地、储存方法等，最多添加50条</span>
                    </el-form-item>
                    <!-- 商品属性 -->
                    <el-form-item class="good_attr">
                        <div class="add_attr" >
                            <el-form ref="attr" label-width="100px" >
                                <el-form-item label="">
                                    <el-col :span="24"  v-for='(attrnum,index) in property' :key="index" >
                                        <el-col :span="4">
                                            <el-input size="small" placeholder="属性名" v-model="attrnum.propertyName" ></el-input>
                                        </el-col>
                                        <el-col :span="16" style="margin-left:10px">
                                            <el-input size="small" placeholder="属性值" v-model="attrnum.propertyValue" ></el-input>
                                        </el-col>
                                        <i class="el-icon-close" style="margin-left:10px" @click="removeattr(index)"></i>
                                    </el-col>
                                </el-form-item>
                            </el-form>
                        </div>
                        <div class="add_button">
                            <el-button type="text" @click="pushAttr">添加属性</el-button>
                        </div>
                    </el-form-item>
                    <!-- 商品主图 -->
                    <el-form-item class="good_title isrequiredClass">
                        <p class="goodtitle_p">商品主图</p>
                        <span class="position" style="width:470px">主图大小不能超过1MB;尺寸：700*700px;最多5张</span>
                    </el-form-item>
                    <!-- 图片上传 -->
                    <el-form-item class="pic_up">
                        <div class="updata_box">
                            <el-upload
                                :action="upDateImgUrl"
                                :data='sentData'
                                :file-list='fileListAdd'
                                multiple
                                list-type="picture-card"
                                :on-preview="handlePictureCardPreview"
                                :on-remove="handleRemove"
                                :on-success='handleAvatarSuccess'
                                :limit= 5>
                                <i class="el-icon-plus"></i>
                            </el-upload>
                            <!-- action上传地址 -->
                            <el-dialog :visible.sync="dialogVisible">
                                <img width="100%" :src="dialogImageUrl" alt="">
                            </el-dialog>
                        </div>
                    </el-form-item>
                    <!-- 商品详情title -->
                    <el-form-item class="good_title isrequiredClass">
                        <p class="goodtitle_p">商品详情</p>
                    </el-form-item>
                    <!-- 富编辑器 -->
                    <el-form-item :style="{background: '#ffffff'}">
                        <div style="padding:10px;background:white;width:80%" class="wangdiv">
                            <div id='editor'></div>
                        </div>
                    </el-form-item>
                    <!-- 其他设置title -->
                    <el-form-item class="good_title">
                        <p class="goodtitle_p">其他设置</p>
                    </el-form-item>
                    <!-- 其他设置 -->
                    <el-form-item class="other">
                        <el-form  label-width="100px" v-model="form">
                            <el-form-item label="报价设置"  style="margin:0" class="eltag">
                                <!-- <el-select @change="setmoneyChange" v-model="setmoneny" placeholder="选择类型" size='small'>
                                    <el-option
                                        v-for="item in setmonenyoptions"
                                        :key="item.id"
                                        :label="item.groupName"
                                        :value="item.id">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item >
                                <el-tag size="small"  style="font-size:12px;background:#409EFF;color:white" v-if='setmoneny'>{{setmonenyWord}}</el-tag> -->
                                <el-select
                                    v-model="setmoneny"
                                    multiple
                                    filterable
                                    allow-create
                                    default-first-option
                                    placeholder="请选择报价类型"
                                    size='small'>
                                    <el-option
                                        v-for="item in setmonenyoptions"
                                        :key="item.id"
                                        :label="item.groupName"
                                        :value="item.id">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                            <el-form-item label="商品热度" >
                                <el-input v-model="pageView" size='small' style="width:215px"></el-input>
                            </el-form-item>
                            <el-form-item label="允许竞价">
                                <el-radio-group v-model="form.isBidding">
                                    <el-radio label = '1' >允许</el-radio>
                                    <el-radio label = '0' >不允许</el-radio>
                                </el-radio-group>
                            </el-form-item>
                            <el-form-item label="出售状态">
                                <el-radio-group v-model="form.onShelf">
                                    <el-radio label = '1'>出售</el-radio>
                                    <el-radio label = '0'>禁售</el-radio>
                                </el-radio-group>
                                <span style="font-size:12px;color:red;margin-left:20px">禁售状态的商品将导致所有在售的商品都会下架，供应商也不能对此商品进行报价</span>
                            </el-form-item>
                            <el-form-item label="减库存设置">
                                <el-radio-group v-model="form.stockWay">
                                    <el-radio label = '0'>下单减库存</el-radio>
                                    <el-radio label = '1'>支付减库存</el-radio>
                                </el-radio-group>
                            </el-form-item>
                            <el-form-item label="提成设置">
                                <el-select
                                    @change="choosePromote" v-model="promoteGroupId" placeholder="选择类型" size='small'>
                                    <el-option
                                        v-for="item in deductoptions"
                                        :key="item.promoteGroupId"
                                        :label="item.promoteGroupName"
                                        :value="item.promoteGroupId">
                                    </el-option>
                                </el-select>
                            </el-form-item>
                        </el-form>
                    </el-form-item>
                </el-form>
            </div>
        </section>
        <!-- 控制步骤 -->
        <div class="add_bottom">
            <!-- <el-button @click="reset" size='small' style="width:90px">重置</el-button> -->
            <el-button @click="next" size='small' type="primary" style="width:90px">保存</el-button>
            <el-button  size='small' style="width:90px" @click="returnprev">取消</el-button>
        </div>
    </section>

</template>
<script>
// import updata from '@/components/updata'
import api from 'api/goods'
import WangEditor from 'wangeditor'
import axios from 'axios'

export default {
    name: 'addGoods',
    data () {
        return {
            typesnameFlag: false,
            value: '',
            editor: null,
            itemSkuPropertyList: [],
            showSkuList: [],
            showSkuIds: [],
            newShowSkuList: [],
            // 验证
            rules: {
                title: [
                    { required: true, message: '请输入商品名称', trigger: 'blur' }
                ],
                barCode: [
                    { required: true, message: '请输入商品条码', trigger: 'blur' }
                ],
                itemCode: [
                    { required: true, message: '请输入商品编号', trigger: 'blur' }
                ],
                brandId: [
                    { required: true, message: '请选择商品品牌', trigger: 'blur' }
                ],
                categoryId: [
                    { required: true, message: '请选择商品分类', trigger: 'blur' }
                ],
                sendWay: [
                    { required: true, message: '请选择贸易类型', trigger: 'blur' }
                ]
            },

            // 商品主图
            dialogImageUrl: '',
            dialogVisible: false,
            dialogImageUrl2: '',
            dialogVisible2: false,

            upDateImgUrl: process.env.API_ROOT + '/f/upload',
            sentData: {
                file: '',
                uploadType: 'erp'
            },
            fileListAdd: [],
            conTinue:[
                {
                    fileList:[
                        {
                            status:'',
                            uid:'',
                            url:''
                        }
                    ]
                }
            ],

            // 信息内容
            form: {
                itemType: 1,
                isPresent: 0,
                title: '',
                sellingPoint: '',
                buyRemind: '',
                barCode: '',
                itemCode: '',
                brandId: [],
                categoryId: [],
                sendWay: [],
                suggestedPrice: '',
                unitId: [],
                expirationDateName: '',
                expirationDateStatus: false,
                stockWay: '',
                isBidding: '',
                onShelf:''
            },
            props: {
                label:'categoryName',
                value:'id',
                children: 'itemCategories'
            },
            // 信息表单选项
            brandIdoptions:[],
            categoryIdoptions:[],
            sendWayoptions:[
                {
                    id:1,
                    categoryName: '国内贸易'
                },
                {
                    id:2,
                    categoryName: '保税区直供'
                },
                {
                    id:3,
                    categoryName: '海外直邮'
                }
            ],
            unitIdoptions:[],
            itemImgs1:[],

            // 商品规格
            checked: false,
            // 规格值
            uploadshow: false,
            skuList: [],
            showSkuArr: [],
            allList_0: [],
            allList_1: [],
            allList_2: [],
            skuProperty: [],

            // 属性表单
            property: [
                {
                    propertyName: '',
                    propertyValue: ''
                }
            ],

            // 内容2

            // 包装类型
            typenameoptions:[],
            typesname: '',
            typevalueoptions:[],
            typesvalue: [
                {
                    id:''
                }
            ],

            // 报价提成
            setmonenyoptions: [],
            setmoneny:[],
            setmonenyWord: '',
            deductoptions:[],
            deduct: '',

            // 其他设置
            promoteGroupId: '',
            promoteGroupName: '',
            pageView: '',
            adddisabled: false,
            requshow: false

        }
    },
    methods:{
        // 下一步保存数据请求接口
        checkboxChangeEvent(data){
            // this.$refs.editGoodsExpReset.clearValidate()
            this.form.expirationDateStatus = data
            this.requshow = data
        },
        next() {
            var id = this.$store.state.home.currentModelId

            this.itemImgs1 = []

            if (this.form.expirationDateStatus == true){
                this.rules.expirationDateName = [
                    { required: true, message: '请输入保质期限', trigger: 'blur' }
                ]
            } else {
                this.rules.expirationDateName = []
            }

            this.$refs['editGoodsForm'].validate((valid) => {
                if (valid) {
                    // 判断商品类型
                    var that = this

                    $(".goodbutton").each(function(i){
                        if ($(".goodbutton").eq(i).find('.jiao').hasClass('active')){
                            if ($(".goodbutton").eq(i).find('p').text() == '实物类商品'){
                                // alert(1)
                                that.form.itemType = 0
                            }
                            if ($(".goodbutton").eq(i).find('p').text() == '服务类商品'){
                                // alert(2)
                                that.form.itemType = 1
                            }
                            if ($(".goodbutton").eq(i).find('p').text() == '赠品'){
                                // alert(3)
                                that.form.isPresent = 1
                            }
                        }
                    });

                    if (this.fileListAdd.length > this.itemImgs1.length){
                        for (var s = 0 ; s < this.fileListAdd.length ; s ++){
                            let obj = {
                                imgUrl:this.fileListAdd[s].url
                            }

                            this.itemImgs1.push(obj)
                        }
                    }

                    var skuProperty = [
                        {
                            id: '',
                            skuPropertyName: '',
                            itemSkuPropertyValueDTOS: [
                                {
                                    id:'',
                                    skuPropertyValueName: ''
                                }
                            ]
                        }
                    ]

                    if (this.skuProperty.length >= skuProperty.length){
                        for (var v = 0 ; v < this.skuProperty.length - 1 ; v ++){
                            let obj = {
                                id: '',
                                skuPropertyName: '',
                                itemSkuPropertyValueDTOS: [
                                    {
                                        id:'',
                                        skuPropertyValueName: ''
                                    }
                                ]
                            }

                            skuProperty.push(obj)
                        }
                    }

                    for (let r in this.skuProperty){
                        if (this.skuProperty[r].itemSkuPropertyValueDTOS.length >= skuProperty[r].itemSkuPropertyValueDTOS.length){
                            for (var f = 0 ; f < this.skuProperty[r].itemSkuPropertyValueDTOS.length - 1 ; f ++){
                                let obj = {
                                    id:'',
                                    skuPropertyValueName: ''
                                }

                                skuProperty[r].itemSkuPropertyValueDTOS.push(obj)
                            }
                        }
                    }

                    var skuPropertyFlg = false

                    for (var t in skuProperty){
                        skuProperty[t].skuPropertyName = this.skuProperty[t].skuPropertyName

                        if (skuProperty[t].skuPropertyName == "") {
                            skuPropertyFlg = true
                        }
                        skuProperty[t].id = this.skuProperty[t].id
                        for (var c in skuProperty[t].itemSkuPropertyValueDTOS){
                            skuProperty[t].itemSkuPropertyValueDTOS[c].skuPropertyValueName = this.skuProperty[t].itemSkuPropertyValueDTOS[c].skuPropertyValueName
                            skuProperty[t].itemSkuPropertyValueDTOS[c].id = this.skuProperty[t].itemSkuPropertyValueDTOS[c].id
                        }
                    }

                    if (skuPropertyFlg) {
                        this.$message({
                            type: 'warning',
                            duration: 1500,
                            showClose: true,
                            message: '请选择商品规格!'
                        });
                        return
                    }

                    for (let z in this.conTinue){

                        skuProperty[0].itemSkuPropertyValueDTOS[z].skuImg = this.conTinue[z].url

                    }

                    for (var m in this.skuList){
                        for (var l in skuProperty){
                            if (skuProperty[l].skuPropertyName == this.skuList[m].skuPropertyName){
                                skuProperty[l].id = this.skuList[m].id
                                for (let p in this.skuList[m].propertyValueList){
                                    for (let y in skuProperty[l].itemSkuPropertyValueDTOS){
                                        if (skuProperty[l].itemSkuPropertyValueDTOS[y].skuPropertyValueName == this.skuList[m].propertyValueList[p].skuPropertyValueName){
                                            skuProperty[l].itemSkuPropertyValueDTOS[y].id = this.skuList[m].propertyValueList[p].id
                                        }
                                    }
                                }

                            }
                        }
                    }

                    if (this.form.expirationDateStatus == false){
                        this.form.expirationDateStatus = 0
                    } else {
                        this.form.expirationDateStatus = 1
                    }

                    var supplyProperty = [
                        {
                            id: this.typesname,
                            itemSupplyPropertyDTOS: [
                                {
                                    id:''
                                }
                            ]
                        }
                    ]

                    if (this.typesvalue.length >= supplyProperty[0].itemSupplyPropertyDTOS.length){
                        for (let r = 0 ; r < this.typesvalue.length - 1 ; r ++){
                            let obj =  {
                                id: ''
                            }

                            supplyProperty[0].itemSupplyPropertyDTOS.push(obj)
                        }
                    }

                    for (let u in this.typesvalue){
                        supplyProperty[0].itemSupplyPropertyDTOS[u].id = this.typesvalue[u].id
                    }
                    // 包装类型判空
                    if (!supplyProperty[0].id) {
                        this.$message({
                            type: 'warning',
                            message: '请选择包装类型'
                        })
                        return
                    }

                    // var propertyFlg = false

                    // this.property.forEach((item, index) => {
                    //     if (item.propertyName == '' || item.propertyValue == '') {
                    //         propertyFlg = true
                    //     }
                    // })

                    // if (propertyFlg) {
                    //     this.$message({
                    //         type: 'warning',
                    //         duration: 1500,
                    //         showClose: true,
                    //         message: '请填写商品属性!'
                    //     });
                    //     return
                    // }


                    if (this.itemImgs1.length == 0) {
                        this.$message({
                            type: 'warning',
                            duration: 1500,
                            showClose: true,
                            message: '请选择商品图片!'
                        });
                        return
                    }

                    let obj = {
                        // 商品信息
                        id: id,
                        title: this.form.title,
                        itemCode: this.form.itemCode,
                        barCode: this.form.barCode,
                        mainImg:this.itemImgs1[0].imgUrl,
                        // 商品属性
                        property: JSON.stringify(this.property),
                        // 商品描述
                        description: this.form.description,
                        sellingPoint: this.form.sellingPoint,
                        buyRemind: this.form.buyRemind,
                        expirationDate: this.form.expirationDateName,
                        expirationDateStatus:this.form.expirationDateStatus,
                        tags:'',
                        brandId: this.form.brandId,
                        categoryId: this.form.categoryId[this.form.categoryId.length - 1],
                        unitId: this.form.unitId,
                        onShelf:this.form.onShelf,
                        itemType: this.form.itemType,
                        isPresent: this.form.isPresent,
                        sendWay: this.form.sendWay,
                        // 商品图片
                        itemImgs: JSON.stringify(this.itemImgs1),
                        // 商品规格
                        skuProperty: JSON.stringify(skuProperty),
                        supplyProperty:JSON.stringify(supplyProperty),
                        suggestedPrice: this.form.suggestedPrice,
                        isBidding:this.form.isBidding,
                        // 其他设置
                        // itemSale:JSON.stringify(this.itemSale),
                        promoteGroupId: this.promoteGroupId,
                        promoteGroupName: this.promoteGroupName,
                        pageView: this.pageView,
                        // 报价
                        supplierAccess:this.setmoneny.toString(),
                        // 减少形式
                        stockWay:this.form.stockWay
                    }

                    // console.log(obj)
                    api.putitemupdate(obj).then((response)=>{
                        this.$message({
                            type: 'success',
                            message: '编辑商品成功！'
                        })
                        this.$router.push({
                            path: '/goodsList'
                        });
                    })

                }
            })
        },
        // 上传图片
        // 移除
        handleRemove(file, fileList) {
            this.fileListAdd = JSON.parse(JSON.stringify(fileList))
        },
        // 变大查看
        handlePictureCardPreview(file) {
            this.dialogImageUrl = file.url;
            this.dialogVisible = true;
        },
        // 成功回调
        handleAvatarSuccess(file, fileList) {
            this.fileListAdd.push(fileList.response.data)
        },
        // 规格图片移除
        smallRemove(file, fileList) {
            for (var i in this.conTinue){
                if (this.conTinue[i].url == file.url){

                    this.conTinue.splice(i, 1)
                }
            }
            if (this.conTinue.length < 5){
                $('.el-upload--picture-card').css('display', 'inline-block')
            }
        },
        // 变大查看
        smallPictureCardPreview(file) {
            this.dialogImageUrl2 = file.url;
            this.dialogVisible2 = true;
        },
        // 成功回调
        smallAvatarSuccess(file, fileList) {
            this.conTinue.push(fileList.response.data)
            if (this.conTinue.length == 5){
                $('.el-upload--picture-card').css('display', 'none')
            }
        },
        // 重置
        reset() {
            this.clear()
            this.$refs['editGoodsForm'].resetFields();
        },
        returnprev() {
            this.clear()
            this.$refs['editGoodsForm'].resetFields();
            this.$router.go(-1)
        },
        // 添加规格
        add() {
            if (this.typevalueoptions.length >= this.typesvalue.length){
                let obj = {
                    id:''
                }

                for (var e in this.typevalueoptions){
                    this.typevalueoptions[e].disabled = false
                }

                for (var i in this.typesvalue){
                    for (var j in this.typevalueoptions){
                        if (this.typesvalue[i].id == this.typevalueoptions[j].id){

                            this.typevalueoptions[j].disabled = true
                        }
                    }
                }
                this.typesvalue.push(obj)
            }

        },
        // 删除规格
        delet(index){
            for (var j in this.typevalueoptions){
                if (this.typesvalue[index].id == this.typevalueoptions[j].id){
                    this.typevalueoptions[j].disabled = false
                }
            }
            if (this.typesvalue.length != 1) {
                this.typesvalue.splice(index, 1)
            }

        },
        typechange(index) {
            let arr = []

            for (var j in this.typevalueoptions){
                if (this.typesvalue[index].id == this.typevalueoptions[j].id){
                    this.typevalueoptions[j].disabled = true
                    this.typesvalue[index].supplyMsg = this.typevalueoptions[j].supplyMsg
                }
            }
            // 规格排序
            this.typevalueoptions.forEach((obj) => {
                this.typesvalue.forEach((res) => {
                    if (res.id == obj.id) {
                        arr.push(res)
                    }
                })
            })

            this.typesvalue = arr
            // console.log(this.typevalueoptions, "包装类型规格")

        },

        // 添加属性
        pushAttr() {
            let obj = {
                propertyName: '',
                propertyValue: ''
            }

            this.property.push(obj)
        },
        // 移除属性
        removeattr(data) {
            if (this.property.length > 1){
                this.property.splice(data, 1)
            }
        },
        // 顶部分类样式
        option($event) {
            var type = $event.currentTarget

            console.log(type)

            $(type).siblings().removeClass('border').find('.jiao').removeClass('active')
            $(type).addClass('border').find('.jiao').addClass('active')
        },
        // 表格数据
        // 添加规格
        addSpec(index) {
            let obj = {
                skuPropertyValueName: '',
                skuImg: '',
                id: '',
                showClose: true
            }

            this.skuProperty[index].itemSkuPropertyValueDTOS.push(obj)

        },
        // 删除规格值
        delskuname(index, pIndex) {
            if (this.skuProperty[index].itemSkuPropertyValueDTOS.length > 1){
                this.skuProperty[index].itemSkuPropertyValueDTOS.splice(pIndex, 1)
            }
            this.updateShowArr()
        },
        // 更新规格展示
        updateShowArr() {
            this.allList_0 = []
            this.allList_1 = []
            this.allList_2 = []
            if (this.skuProperty[0]) {
                this.skuProperty[0].itemSkuPropertyValueDTOS.forEach((res) => {
                    // this.allList_0.push(res.skuPropertyValueName)
                    this.allList_0.push({'id': res.id, 'name': res.skuPropertyValueName})
                })
            }
            if (this.skuProperty[1]) {
                this.skuProperty[1].itemSkuPropertyValueDTOS.forEach((res) => {
                    this.allList_1.push({'id': res.id, 'name': res.skuPropertyValueName})
                })
            }
            if (this.skuProperty[2]) {
                this.skuProperty[2].itemSkuPropertyValueDTOS.forEach((res) => {
                    this.allList_2.push({'id': res.id, 'name': res.skuPropertyValueName})
                })
            }
            this.showSkuArr = []
            this.showSkuIds = []
            if (this.allList_2.length > 0) {
                for (let i = 0; i < this.allList_0.length; i++) {
                    for (let j = 0; j < this.allList_1.length; j++) {
                        for (let k = 0; k < this.allList_2.length; k++) {
                            this.showSkuArr.push({'name': [this.allList_0[i].name, this.allList_1[j].name, this.allList_2[k].name], 'skuId': '', 'id': [this.allList_0[i].id, this.allList_1[j].id, this.allList_2[k].id]})
                            this.showSkuIds.push([this.allList_0[i].id, this.allList_1[j].id, this.allList_2[k].id])
                        }
                    }
                }
            } else if (this.allList_1.length > 0) {
                for (let i = 0; i < this.allList_0.length; i++) {
                    for (let j = 0; j < this.allList_1.length; j++) {
                        this.showSkuArr.push({'name': [this.allList_0[i].name, this.allList_1[j].name], 'skuId': '', 'id': [this.allList_0[i].id, this.allList_1[j].id]})
                        this.showSkuIds.push([this.allList_0[i].id, this.allList_1[j].id])
                    }
                }
            } else if (this.allList_0.length > 0) {
                for (let i = 0; i < this.allList_0.length; i++) {
                    this.showSkuArr.push({'name': [this.allList_0[i].name], 'skuId': '', 'id': [this.allList_0[i].id]})
                    this.showSkuIds.push([this.allList_0[i].id])
                }
            }
            this.newShowSkuList = []
            // 对生成的id进行排序
            this.showSkuIds.forEach((sigSku, index) => {
                this.showSkuList.forEach((sku, index2) => {
                    if (sku.skuProperties == JSON.stringify(sigSku)) {
                        this.newShowSkuList.push(sku)
                    }
                })
                console.log(JSON.stringify(sigSku), index, "dds")
            })
            // 将skuId对应的赋值给showSkuArr
            this.showSkuArr.forEach((skuArrRes) => {
                this.newShowSkuList.forEach((skuListRes) => {
                    if (skuListRes.skuProperties == JSON.stringify(skuArrRes.id)) {
                        skuArrRes.skuId = skuListRes.id
                    }
                })
            })
            console.log(this.showSkuArr, this.newShowSkuList, "dsdasdsadasd")
        },
        // 修改规格值
        changeSKUname(data, index) {
            data.itemSkuDTOS.forEach((obj) => {
                obj.disabled = false
            })
            // 已经选择的规格值
            data.itemSkuPropertyValueDTOS.forEach((res) => {
                data.itemSkuDTOS.forEach((obj) => {
                    if (res.skuPropertyValueName == obj.skuPropertyValueName) {
                        obj.disabled = true
                    }
                })
            })
            this.updateShowArr()

            // console.log(this.showSkuArr, 'sku总列表展示')
            // console.log(this.skuProperty, 'sku传递给后台数据')
        },
        // 修改规格名去重
        changeSku(data, index, skuList) {
            this.skuList.forEach((res) => {
                res.disabled = false
            })
            // console.log(this.skuProperty)
            this.skuList.forEach((res) => {
                this.skuProperty.forEach((obj) => {
                    if (res.skuPropertyName == obj.skuPropertyName) {
                        res.disabled = true
                    }
                })

            })
            data.itemSkuPropertyValueDTOS = [{
                id: '',
                skuImg: '',
                skuPropertyValueName: '',
                showClose: true
            }]
            this.$set(data, 'itemSkuDTOS', [])
            api.getitemskuPropertylist({skuPropertyName: data.skuPropertyName}).then((res) => {
                // console.log(res)
                res.data.list[0].propertyValueList.forEach((re) => {
                    let obj = {
                        skuPropertyValueName: re.skuPropertyValueName,
                        skuImg: '',
                        Id: re.id
                    }

                    data.itemSkuDTOS.push(obj)
                })
            })
        },
        // 获取规格列表
        getSkuList() {
            api.getitemskuPropertylist().then((res) => {
                this.skuList = res.data.list
                // console.log(this.skuList)
            })
        },
        // 添加规格项目
        addSpecItem() {
            if (this.skuProperty.length < 3){
                let obj = {
                    id:'',
                    skuPropertyName: "",
                    itemSkuPropertyValueDTOS :[
                        {
                            skuPropertyValueName: "",
                            skuImg: '',
                            id: '',
                            showClose:true
                        }
                    ],
                    showClose: true
                }

                this.skuProperty.push(obj)
                // this.skuProperty[length - 1].itemSkuPropertyValueDTOS[0].showClose = false
            }
            if (this.skuProperty.length == 3){
                this.adddisabled = true
            }

        },
        // 删除规格项目
        delskuproperty(index) {
            this.adddisabled = false
            if (this.skuProperty.length > 1){
                this.skuProperty.splice(index, 1)
            }
            this.updateShowArr()
        },
        changeupload() {
            this.uploadshow = !this.uploadshow
        },

        // 获取商品品牌选项
        getbrandId() {
            let obj = {
                pageNo: 1,
                pageSize: 999999
            }

            api.getitemBrandlist(obj).then((response)=>{
                // console.log(response)
                this.brandIdoptions = response.data.list
            })
        },
        // 获取商品分类选项
        getcategoryId() {
            api.getcategorylist().then((response)=>{
                // console.log(response)
                this.categoryIdoptions = response.data
            })
        },
        // 获取单位选项
        getunitId() {
            api.getitemunitlist().then((response)=>{
                this.unitIdoptions = response.data.list
                // console.log(response)
            })
        },
        // 获取包装类型规格
        gettypename() {
            let obj = {
                pageNo: 1,
                pageSize: 50
            }

            api.getitemsupplyPropertylist(obj).then((response)=>{
                // console.log(response)
                this.typenameoptions = response.data.list
            })
        },
        gettypevalue() {
            this.typesvalue = [
                {
                    id:''
                }
            ]
            api.getitemsupplyPropertyunitId(this.typesname).then((response)=>{
                // console.log(response)
                this.typevalueoptions = response.data.itemSupplyPropertyVOs
            })
        },
        // 获取报价设置
        getgroup() {
            let page = {
                pageNo: 1,
                pageSize: 99
            }

            api.getsuppliergrouplist(page).then((response)=>{
                // console.log(response)
                this.setmonenyoptions = response.data.list
            })
        },
        // 获取提成设置
        getPromotesList() {
            api.getitempromotes().then(res => {
                this.deductoptions = res.data.list
            })
        },
        // 选择提成设置
        choosePromote() {
            this.deductoptions.forEach((res) => {
                if (res.promoteGroupId == this.promoteGroupId) {
                    this.promoteGroupName = res.promoteGroupName
                }
            })
        },
        clear() {
            this.form = {
                itemType: 1,
                isPresent: 0,
                title: '',
                sellingPoint: '',
                buyRemind: '',
                barCode: '',
                itemCode: '',
                money: '',
                expirationDateName: '',
                expirationDateStatus: false,
                goodhot:'',
                stockWay: '',
                isBidding: '',
                onShelf:''
            }
            this.property = [
                {
                    propertyName: '',
                    propertyValue: ''
                }
            ]
            this.typesvalue = [
                {
                    id:''
                }
            ]
            this.typesname = ''
            this.setmoneny = []
            this.deduct = ''
            this.skuProperty = []
            this.fileListAdd = []
            this.checked = false
            this.uploadshow = false
            this.skuList = []
            this.showSkuArr = []
            this.allList_0 = []
            this.allList_1 = []
            this.allList_2 = []
            this.adddisabled = false
            this.requshow = false
        },
        get() {
            this.getbrandId()
            this.getcategoryId()
            this.gettypename()
            this.getunitId()
            this.getgroup()
            this.getSkuList()
            this.getPromotesList()
        },
        getgoods() {
            var id = this.$store.state.home.currentModelId

            api.getitemedititemId(id).then((response)=>{
                // console.log(response)
                this.form = response.data.item
                this.form.categoryId = []
                var categoryIdsArr = this.form.categoryIds.split(',')

                categoryIdsArr.forEach(num => {
                    this.form.categoryId.push(parseInt(num, 10))
                })
                // console.log(categoryIdsArr, "目录ID")
                this.form.stockWay = response.data.item.stockWay.toString()
                this.form.isBidding = response.data.item.isBidding.toString()
                this.form.onShelf = response.data.item.onShelf.toString()
                this.promoteGroupId = this.form.promoteGroupId
                this.property = JSON.parse(response.data.item.property)

                // console.log(this.property)
                $(".parent").removeClass('border').find('.jiao').removeClass('active')

                //根据商品列表页面的tab页,确定当前显示类型wwb
                if (this.$route.params.type == 0){
                    $(".parent").eq(0).addClass('border').find('.jiao').addClass('active')
                }
                if (this.$route.params.type == 1){
                    $(".parent").eq(1).addClass('border').find('.jiao').addClass('active')
                }
                if (this.$route.params.type == 2){
                    $(".parent").eq(2).addClass('border').find('.jiao').addClass('active')
                }

                if (this.form.expirationDateStatus == 1){
                    this.form.expirationDateStatus = true
                    this.requshow = true
                } else {
                    this.form.expirationDateStatus = false
                    this.requshow = false
                }

                // console.log(this.form)
                var list = response.data.itemSkuPropertyList

                this.itemSkuPropertyList = list

                // 商品规格获取
                if (list.length >= this.skuProperty.length){

                    for (var i = 0 ; i < list.length ; i ++){
                        let obj = {
                            id:'',
                            skuPropertyName: "",
                            itemSkuPropertyValueDTOS :[
                                {
                                    skuPropertyValueName: "",
                                    skuImg: '',
                                    id: ''
                                }
                            ]
                        }

                        this.skuProperty.push(obj)
                    }
                }

                for (let a in list){
                    if (list[a].propertyValueList.length >= this.skuProperty[a].itemSkuPropertyValueDTOS.length){
                        for (var b = 0 ; b < list[a].propertyValueList.length - 1 ; b ++){
                            let objj = {
                                skuPropertyValueName: "",
                                skuImg: '',
                                id: ''
                            }

                            this.skuProperty[a].itemSkuPropertyValueDTOS.push(objj)
                        }
                    }
                }

                // 规格属性赋值去重
                for (let q in list){
                    this.skuProperty[q].id = list[q].id
                    this.skuProperty[q].skuPropertyName = list[q].skuPropertyName
                    for (var w in list[q].propertyValueList){
                        this.skuProperty[q].itemSkuPropertyValueDTOS[w].skuPropertyValueName = list[q].propertyValueList[w].skuPropertyValueName
                        this.skuProperty[q].itemSkuPropertyValueDTOS[w].id = list[q].propertyValueList[w].id

                        this.$set(this.skuProperty[q], 'itemSkuDTOS', [])
                        api.getitemskuPropertylist({skuPropertyName: this.skuProperty[q].skuPropertyName}).then((res) => {

                            res.data.list[0].propertyValueList.forEach((re) => {
                                let obj = {
                                    skuPropertyValueName: re.skuPropertyValueName,
                                    skuImg: '',
                                    Id: re.id,
                                    disabled: false
                                }

                                this.skuProperty[q].itemSkuDTOS.push(obj)

                                for (let p in this.skuProperty[q].itemSkuDTOS){
                                    if (this.skuProperty[q].itemSkuPropertyValueDTOS[w].id == this.skuProperty[q].itemSkuDTOS[p].id){
                                        this.skuProperty[q].itemSkuDTOS[p].disabled = true
                                    }
                                }
                            })
                        })

                    }
                }

                // 规格小块去重
                for (var n in this.skuList){
                    this.skuList[n].disabled = false
                    for (var m in this.skuProperty){
                        if (this.skuProperty[m].skuPropertyName == this.skuList[n].skuPropertyName){
                            this.skuList[n].disabled = true
                        }
                    }
                }

                // 商品属性获取
                this.typesname = response.data.itemUnitVO.id
                this.typesvalue = response.data.itemUnitVO.itemSupplyPropertyVOs
                console.log(!this.typesname, "包装类型")
                if (!this.typesname) {
                    this.typesnameFlag = false
                } else {
                    this.typesnameFlag = true
                    api.getitemsupplyPropertyunitId(this.typesname).then((res)=>{
                        this.typevalueoptions = res.data.itemSupplyPropertyVOs
                        for (let t in this.typesvalue){
                            for (let j in this.typevalueoptions){
                                if (this.typevalueoptions[j].id == this.typesvalue[t].id){
                                    this.typevalueoptions[j].disabled = true
                                    this.typesvalue[t].supplyMsg = this.typevalueoptions[j].supplyMsg
                                }
                            }
                            this.typesvalue[t].showCloseFlag = true
                        }
                    })
                }

                // 规格明细表单

                // 商品图片获取
                if (response.data.itemImgs.length > this.fileListAdd.length){
                    for (let l = 0 ; l < response.data.itemImgs.length; l ++){
                        let obj = {
                            status:'',
                            uid:'',
                            url:''
                        }

                        this.fileListAdd.push(obj)
                    }
                }
                for (let u in response.data.itemImgs){
                    this.fileListAdd[u].url = response.data.itemImgs[u].imgUrl
                }

                if (this.fileListAdd.length == 5){
                    $('.el-upload--picture-card').css('display', 'none')
                }
                // 报价设置
                this.pageView = response.data.item.pageView

                for (let x in response.data.supplierAccess){
                    this.setmoneny.push(response.data.supplierAccess[x].supplierGroupId)
                }

                this.editor.txt.html(response.data.item.description)
                // 获取sku编码列表
                this.showSkuList = response.data.itemSkuList
                this.showSkuList.forEach((obj) => {
                    let arr = obj.skuProperties.replace('[', '').replace(']', '').split(',')
                    let newArr = []

                    arr.forEach((ArrNum) => {
                        console.log(ArrNum.replace('[', '').replace(']', '').split(':')[1], '数值是多少')
                        newArr.push(parseInt(ArrNum.replace('[', '').replace(']', '').split(':')[1], 10))
                    })
                    console.log(arr, newArr, "JSON专属在2")
                    obj.skuProperties = JSON.stringify(newArr)
                })
                this.updateShowArr()
            })


        }

    },
    created() {
        this.get()
    },
    activated() {
        this.clear()
        this.get()
        this.getgoods()
        this.$refs['editGoodsForm'].resetFields();
    },
    mounted() {
        this.editor = new WangEditor('#editor')
        this.editor.customConfig.customUploadImg = function(files, insert) {
            let formData = new FormData()

            formData.append('file', files[0])
            formData.append('uploadType', 'erp')

            axios.post('/f/upload', formData).then(res => {
                insert(res.data.data.url)
                // console.log(res.data.data.url)
            })
            // insert(imgUrl)
        }
        this.editor.customConfig.onchange = (html) => {
            this.form.description = html
        }
        this.editor.create()
        $('.w-e-text-container').height(600)
    }
}
</script>
<style scoped>
    .jiao{
        position: absolute;
        height: 0px;
        width: 0px;
        border-top: 0px solid transparent;
        border-right: 0px solid #3ea0fc;
        border-bottom: 20px solid #3ea0fc;
        border-left: 20px solid transparent;
        right: 0;
        bottom: 0;
        color: white;
        display: none
    }
    .active{
        display: block;
    }
    .border{
        border: 1px solid #3ea0fc;
        color: #3ea0fc
    }
    #add{
        position: relative;
        height: 949px
    }
    /* 商品通用标题 */
    .good_title{
        width: 100%;
        height: 40px;
        line-height: 40px;
        color: #5f6264;
        display: flex;
        margin: 0
    }
    .position{
        position: absolute;
        left: 100px;
        top:0;
        font-size: 12px
    }
    .goodtitle_p{
        margin-left: 10px;
        font-weight: 600
    }
    /* 面包屑 */
    .add_top{
        height: 45px;
        width: 100%;
        padding: 0 30px;
    }
    /* 主体 */
    .add_conent{
        padding:10px;
        background: #f5f5f5;
        border-top:1px solid #e6e9e9;
        overflow: auto
    }
    .conent_box{
        border: 1px solid #e6e9e9
    }
    .conent_box .el-form-item{
        margin: 0
    }
    /* 步骤 */
    .step{
        padding: 20px 10px;
        background: white;
    }

    /* 第一步 */

    /* 商品类型 */
    .good_type{
        width: 100%;
        background: white;
        padding: 15px 20px;
        position: relative
    }
    .color{
        color: #5f6264
    }
    /* 信息内容 */
    .msg_conent{
        background: white;
        padding: 10px 40px;
    }
    .msg_conent .el-textarea{
        width: 688px;
    }
    .msg_conent .el-input{
        width: 258px;
    }
    .msg_conent .el-select{
        width: 258px;
    }
    /* 商品规格 */
    .standard{
        background: white;
        padding: 10px 40px;
    }
    .add_standard{
        border: 1px solid #dce0e6;
        width: 100%;
        padding: 10px;
    }
    .add_small_standard{
        background: #f5f5f5;
        height: 50px;
        padding: 3px 10px;
    }
    .el-upload__input{
        display: none
    }
    .standard_p{
        margin-left: 10px;
        color: #5f6264;
    }
    .updata{
        width: 88px;
        height: 88px;
        line-height: 88px;
        border: 1px dashed #888888;
        text-align: center;
        font-size: 26px
    }

    /* 商品属性 */
    .good_attr{
        background: white;
        padding: 10px 40px;
    }
    .add_button{
        margin:0 0 0 100px
    }
    /* 商品主图 */
    .pic_up{
        background: white;
        padding: 10px 40px;
    }

    /* 控制步骤 */
    .add_bottom{
        height: 50px;
        padding: 10px 25px;
        background: white;
        border-top:1px solid #e5e8e8;
    }

    /* 内容2 */

    /* 供货设置 */
    .supply{
        background: white;
        padding: 10px 40px;
        height: 300px
    }
    .supply_check .el-tag{
        background: none;
        border: none;
        padding: 0;
    }
    /* 分享 */
    .share{
        background: white;
        padding: 10px 40px;
        height: 600px
    }
    /* 其他 */
    .other{
        background: white;
        padding: 10px 40px;
        height: 400px
    }
    .share .el-upload--picture-card{
        width: 78px;
        height: 78px;
        line-height: 78px;
    }
    .other .el-tag{
        background: none;
        color: black;
        border: none;
        font-size: 14px
    }
    .other .el-icon-close{
        background: #c0c4ca;
        color: white;
        width: 18px;
        height: 18px;
        line-height: 18px
    }
    .other .el-icon-close:hover{
        font-size: 6px;
        background: #c0c4ca;
    }
    .share .el-form-item {
        margin-bottom: 0;
        height: 140px;
    }
    .skuTable {
        text-align: center;
        font-size: 14px;
        margin-top: 20px;
        margin-bottom: 20px;
        border: 1px solid #eee;
        width: 100%;
    }
    .skuTable .el-input__inner {
        width: 80%;
        height: 30px;
        margin: 5%;
    }
    .skuTable .tableBox {
        display: flex;
        border-top: 1px solid #eee;
    }
    .skuTable .tableBox span {

        flex: 1;
        display: inline-block;
    }
    .skuTable .tableBox tr {
        text-align: center;
        border: 1px solid #eee;
    }
    .skuTable div {
        text-align: center;
        line-height: 50px;
        height: 50px
    }
    .skuTable td {
        border: 1px solid #eee;
        font-weight: 400;
    }
    .skuTable .tableBox2 {
        display: flex;
    }
    .skuTable .tableBox2 span {
        flex: 1;
    }
    .skuTable th {
        text-align: center;
        font-weight: 400;
        height: 50px;
        border: 1px solid #eee;
    }
    .isrequiredClass::before{
        content: "*";
        color: #f56c6c;
        margin-right: -5px;
        margin-left: 5px;
    }

</style>

