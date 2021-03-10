<template>
    <div class="hotel-index">
        <!-- 面包屑导航 -->
        <el-breadcrumb separator-class="el-icon-arrow-right" class="bread-nav">
            <el-breadcrumb-item>酒店</el-breadcrumb-item>
            <el-breadcrumb-item>{{ $store.state.hotel.cityName }}酒店预订</el-breadcrumb-item>
        </el-breadcrumb>

        <!-- 酒店过滤查询 -->
        <!-- <el-form :inline="true" :model="formInline" class="query-form">
            <el-form-item>
                <el-date-picker
                    v-model="formInline.date"
                    type="daterange"
                    start-placeholder="入住时间"
                    end-placeholder="离店时间"
                    range-separator="-"
                    :picker-options="pickerOptions"
                >
                </el-date-picker>
            </el-form-item>
            <el-form-item>
                <div class="hotel-people">
                    <el-popover placement="bottom" width="320" trigger="click" class="popover-box">
                        <span class="hotel-select-item">每间</span>
                        <el-row type="flex" align="middle" class="members-row">
                            <el-select
                                v-model="selectData.valueO"
                                placeholder="2成人"
                                size="mini"
                                class="select-item"
                                @change="addlabel('成人')"
                            >
                                <el-option
                                    v-for="item in 7"
                                    :key="item"
                                    :label="item"
                                    :value="item"
                                ></el-option>
                            </el-select>
                            <el-select
                                v-model="selectData.valueT"
                                placeholder="0儿童"
                                size="mini"
                                class="select-item"
                                @change="addlabel('儿童')"
                            >
                                <el-option
                                    v-for="item in 4"
                                    :key="item + 10"
                                    :label="item"
                                    :value="item"
                                ></el-option>
                            </el-select>
                        </el-row>
                        <div class="btn-col"></div>
                        <el-row type="flex" justify="end">
                            <el-button type="primary" size="mini" @click="addContentToInput"
                                >确定</el-button
                            >
                        </el-row>
                        <div slot="reference" ref="personal">
                            <el-input
                                placeholder="请输入内容"
                                suffix-icon="el-icon-user"
                                readonly="readonly"
                                v-model="formInline.input2"
                            ></el-input>
                        </div>
                    </el-popover>
                </div>
            </el-form-item>
            <el-form-item
                ><el-button type="primary" @click="queryPrice">查看价格</el-button></el-form-item
            >
        </el-form> -->

        <!-- 区域信息 -->
        <el-row type="flex" justify="space-between" :gutter="20">
            <el-col :span="24">
                <el-col :span="8" v-for="item in hotelInfo.data" :key="item.id">
                    <div class="card">
                        <div class="img-box">
                            <img :src="item.image_url" alt="" srcset="" />
                        </div>
                        <div class="detail">
                            <p style="font-weight:bold">{{ item.title }}</p>
                            <p class="address">地址：{{ item.address }}</p>
                            <p class="phone">联系电话：{{ item.phone }}</p>
                            <el-tag
                                class="tag"
                                v-for="tag in tags"
                                :key="tag.name"
                                size="mini"
                                :type="tag.type"
                            >
                                {{ tag.name }}
                            </el-tag>
                            <div class="price">
                                ￥
                                <span style="font-weight:bold;font-size:18px">{{
                                    item.price.toFixed(2)
                                }}</span>
                                元
                            </div>
                        </div>
                    </div>
                </el-col>
            </el-col>
        </el-row>
    </div>
</template>

<script>
import HotelFilter from '@/components/hotel/hotelFilter'
import moment from 'moment'

export default {
    components: {
        HotelFilter
    },
    data() {
        return {
            hotelInfo: [],
            tags: [
                { name: '情侣约会', type: 'success' },
                { name: '安心住', type: 'info' },
                { name: '钟点房', type: 'warning' },
                { name: '免费取消', type: 'primary' }
            ],
            pickerOptions: {
                disabledDate(time) {
                    return time.getTime() < Date.now() - 24 * 3600 * 1000
                }
            },
            selectData: {
                valueO: '2成人',
                valueT: '0儿童'
            },
            formInline: {
                departCity: '',
                input2: ''
            },
            cityData: {},
            cityTemp: [],
            searchData: [],
            map: '',
            infoWindow: '',
            isSpread: true,
            enterTime: '',
            leftTime: '',
            loading: true
        }
    },
    mounted() {
        // 初始化数据
        // this.getAmap()
        this.fetchHotelInfo()
    },
    computed: {
        // scenics() {
        //     if (!this.cityData.scenics) {
        //         if (this.$store.state.hotel.scenics.length) {
        //             return this.$store.state.hotel.scenics
        //         }
        //         return []
        //     }
        //     return this.cityData.scenics
        // }
    },
    methods: {
        async fetchHotelInfo() {
            const result = await this.$axios({
                url: '/sights/hotel',
                params: {
                    count: 9
                }
            })
            this.hotelInfo = result.data
        }
        // async queryDepartSearch(value, callback) {
        //     this.cityTemp = await this.querySearchAsync(this.formInline.departCity)
        //     if (this.cityTemp.length > 0) {
        //         this.formInline.departCity = this.cityTemp[0].value
        //         this.formInline.departCode = this.cityTemp[0].sort
        //         this.cityData = this.cityTemp[0]
        //         this.getSearchData(this.cityData.id)
        //         this.$store.commit('hotel/setCityName', this.cityData.value)
        //         this.$store.commit('hotel/setCityPid', this.cityData.id)
        //         this.$store.commit('hotel/setScenics', this.cityData.scenics)
        //     }
        //     callback(this.cityTemp)
        // },
        // queryPrice() {
        //     this.formInline.date = this.formInline.date.map(v => {
        //         v = moment(v).format('YYYY-MM-DD')
        //         return v
        //     })
        //     this.enterTime = this.formInline.date[0]
        //     this.leftTime = this.formInline.date[1]
        // },
        // spreadContent() {
        //     this.isSpread = !this.isSpread
        // },
        // 地图实例渲染
        // getAmap() {
        //     // window.onLoad = () => {
        //     //   this.map = new AMap.Map('container', {
        //     //     resizeEnable: true,
        //     //     zoom: 13
        //     //   })
        //     //   const cityName = this.$store.state.hotel.cityName
        //     //   if (!cityName) {
        //     //     this.autoLocation()
        //     //   } else {
        //     //     const pid = this.$store.state.hotel.cityPid
        //     //     this.$message.success(`当前所在${cityName}，请手动更改城市`)
        //     //     this.formInline.departCity = cityName
        //     //     pid && this.getSearchData(pid)
        //     //     this.querySearchAsync(cityName)
        //     //   }
        //     // }
        //     var url =
        //         'https://webapi.amap.com/maps?v=1.4.15&key=0bc30017317f7c4cb9192fcc0b735c33&callback=onLoad'
        //     var jsapi = document.createElement('script')
        //     jsapi.charset = 'utf-8'
        //     jsapi.src = url
        //     document.head.appendChild(jsapi)
        // },
        // async getSearchData(id) {
        //     let { data } = await this.$axios({
        //         url: `/hotels`,
        //         params: {
        //             city: id
        //         }
        //     })
        //     this.searchData = data.data
        //     this.setMarker()
        // },
        // autoLocation() {
        //     // 定位插件
        //     AMap.plugin('AMap.CitySearch', () => {
        //         var citySearch = new AMap.CitySearch()
        //         citySearch.getLocalCity((status, result) => {
        //             if (status === 'complete' && result.info === 'OK') {
        //                 // 查询成功，result即为当前所在城市信息
        //                 this.$message.success(`当前已定位到${result.city}`)
        //                 this.formInline.departCity = result.city
        //                 this.querySearchAsync(result.city)
        //             } else {
        //                 this.$message.warning('定位失败，自动跳转到上海市，请手动更改城市')
        //                 this.formInline.departCity = '上海市'
        //                 this.querySearchAsync('上海市')
        //             }
        //         })
        //     })
        // },
        // 设置点标记
        // setMarker() {
        //     this.map = new AMap.Map('container', {
        //         resizeEnable: true,
        //         zoom: 13
        //     })
        //     const lnglats = this.searchData.map(v => {
        //         return [v.location.longitude, v.location.latitude]
        //     })
        //     this.map.clearMap()
        //     this.infoWindow = new AMap.InfoWindow({ offset: new AMap.Pixel(0, -30) })
        //     for (var i = 0, marker; i < lnglats.length; i++) {
        //         var marker = new AMap.Marker({
        //             icon: 'https://webapi.amap.com/theme/v1.3/markers/n/mark_b' + (i + 1) + '.png',
        //             position: lnglats[i],
        //             map: this.map
        //         })
        //         marker.content = this.searchData[i].name
        //         marker.on('mouseover', this.markerClick)
        //         marker.emit('mouseover', { target: marker })
        //     }
        //     this.map.setFitView()
        //     this.loading = false
        // },
        // 鼠标点击地图marker
        // markerClick(e) {
        //     this.infoWindow.setContent(e.target.content)
        //     this.infoWindow.open(this.map, e.target.getPosition())
        // },
        // handleDepartSelect(data) {
        //     if (data.id === this.cityData.id) {
        //         return
        //     }
        //     this.cityData = data
        //     this.getSearchData(this.cityData.id)
        //     this.$store.commit('hotel/setCityName', data.value)
        //     this.$store.commit('hotel/setCityPid', data.id)
        //     this.$store.commit('hotel/setScenics', this.cityData.scenics)
        //     this.formInline.destCity = data.value
        //     this.formInline.destCode = data.sort
        // },
        // querySearchAsync(keywords) {
        //     try {
        //         return new Promise((resolve, reject) => {
        //             if (!keywords || keywords.length === 0) {
        //                 return resolve([])
        //             }
        //             if (keywords === this.$store.state.hotel.cityName) {
        //                 return resolve([])
        //             }
        //             this.$axios({
        //                 url: '/cities',
        //                 params: {
        //                     name: keywords
        //                 }
        //             })
        //                 .then(({ data }) => {
        //                     if (!data.data.length) {
        //                         this.$message.error('未搜索到该城市，试试拼音哦')
        //                         return resolve([])
        //                     }
        //                     if (!this.$store.state.hotel.cityName) {
        //                         this.cityData = data.data[0]
        //                         this.$store.commit('hotel/setCityName', this.cityData.name)
        //                         this.$store.commit('hotel/setCityPid', this.cityData.id)
        //                         this.$store.commit('hotel/setScenics', this.cityData.scenics)
        //                         this.getSearchData(this.cityData.id)
        //                     }
        //                     data = data.data.map(v => {
        //                         return {
        //                             ...v,
        //                             value: v.name
        //                         }
        //                     })
        //                     resolve(data)
        //                 })
        //                 .catch(e => {
        //                     this.$message.error('未搜索到该城市，试试拼音哦')
        //                     return resolve([])
        //                 })
        //         })
        //     } catch (e) {}
        // },
        // addContentToInput() {
        //     this.$refs.personal.click()
        //     if (this.selectData.valueT === '0儿童') {
        //         this.formInline.input2 = this.selectData.valueO
        //         return
        //     }
        //     this.formInline.input2 = this.selectData.valueO + ' ' + this.selectData.valueT
        // },
        // addlabel(data) {
        //     if (data === '成人') {
        //         this.selectData.valueO += data
        //     } else {
        //         this.selectData.valueT += data
        //     }
        // }
    }
}
</script>

<style lang="less" scoped>
.hotel-index {
    width: 1000px;
    padding: 20px;
    min-width: 900px;
    margin: 0 auto;
    .bread-nav {
        padding: 20px 0;
    }
    .card {
        position: relative;
        margin-top: 20px;
        width: 290px;
        height: 370px;
        border-radius: 8px;
        .img-box {
            margin-bottom: 10px;
            width: 290px;
            height: 200px;
            overflow: hidden;
            border-radius: 8px;
        }
        .detail {
            padding: 0 10px 10px 10px;
            line-height: 30px;
            .address,
            .phone {
                font-size: 13px;
                color: #777777;
            }
            .tag {
                margin-right: 10px;
            }
            .price {
                font-size: 14px;
                color: red;
                position: absolute;
                right: 10px;
                bottom: 10px;
            }
        }
    }
    .card:hover {
        box-shadow: 0 0 15px 0 #ddd;
    }

    // .area-map {
    //     .hotel-area .el-icon-d-arrow-right {
    //         color: #f90;
    //         transform: rotate(270deg);
    //         cursor: pointer;
    //     }
    //     .hotel-area .hidden-all .el-icon-d-arrow-right {
    //         transform: rotate(90deg);
    //     }
    //     .limitHeight {
    //         max-height: 3em;
    //         overflow: hidden;
    //     }
    //     .hotel-price-type {
    //         margin-top: 20px;
    //         .hotel-tool {
    //             display: inline-block;
    //             position: relative;
    //             top: -3px;
    //             left: 5px;
    //             width: 20px;
    //             height: 20px;
    //             background-color: #cccccc;
    //             border-radius: 50%;
    //             font-size: 10px;
    //             text-align: center;
    //             line-height: 20px;
    //             color: #fff;
    //             font-weight: 700;
    //         }
    //     }
    // }
}
.select-item {
    width: 100px;
    padding: 5px;
}
</style>
