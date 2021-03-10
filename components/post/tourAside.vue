<template>
    <div class="menus-body" @mouseleave="cgIndexOut">
        <div class="menus">
            <div
                class="menus-circur"
                @mouseover="showTabList(index)"
                @mouseout="hideTabList"
                :style="{
                    color: currentIndex === index ? 'orange' : ''
                }"
                v-for="(item, index) in cityList"
                :key="index"
            >
                <el-row type="flex" justify="space-between" align="middle" class="menu-item">
                    <span>{{ item.type }}</span>
                    <i class="el-icon-arrow-right"></i>
                </el-row>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            cityList: [],
            currentIndex: 4,
            isShow: false,
            time: 0
        }
    },
    computed: {
        tabContent() {
            if (!this.cityList[this.currentIndex]) {
                return []
            }
            return this.cityList[this.currentIndex].children
        }
    },
    mounted() {
        this.getCityData()
    },
    methods: {
        getCityData() {
            this.$axios({
                url: '/travel/nav'
            }).then(({ data }) => {
                this.cityList = data.data
            })
        },
        showTabList(index) {
            this.currentIndex = index
            this.isShow = true
        },
        hideTabList(index) {
            this.isShow = false
        },
        sendCityName(data) {
            this.$emit('gainCityName', data)
        },
        cgIndexOut() {
            this.isShow = false
            this.currentIndex = 4
        },
        hideTabListAll() {
            this.isShow = false
            this.currentIndex = 4
        },
        cgIndex() {
            this.currentIndex = 4
        }
    }
}
</script>

<style lang="less" scoped>
.menus-body {
    position: relative;
}
.menus {
    width: 260px;
    border-bottom: 1px solid #ddd;
    .menus-circur {
        box-sizing: border-box;
        width: 260px;
        height: 41px;
        padding: 0 20px;
        font-size: 14px;
        border: 1px solid #ddd;
        border-bottom: none;

        &:hover {
            border-right: none;
            span {
                color: orange;
            }
            i {
                color: orange;
            }
        }
    }
}
.menu-item {
    width: 100%;
    height: 100%;
}
.sub-menus {
    box-sizing: border-box;
    width: 350px;
    padding: 10px 20px;
    position: absolute;
    right: -319px;
    top: 0;
    border: 1px solid #ddd;
    border-left: none;
    background-color: #fff;
    z-index: 999;
    &::before {
        content: '';
        display: block;
        width: 1px;
        height: 53px;
        background-color: #ddd;
        position: absolute;
        bottom: 0;
        left: -1px;
    }
    ul {
        li {
            vertical-align: middle;
            font-size: 14px;
            left: 36px;
            line-height: 36px;
        }
    }
    i {
        position: relative;
        top: 3px;
        color: orange;
        font-size: 24px;
        font-style: italic;
    }
    strong {
        margin: 0 10px;
        color: orange;
        font-weight: 400;
        cursor: pointer;
        &:hover {
            text-decoration: underline;
        }
    }
    span {
        color: #999;
        cursor: pointer;
        &:hover {
            text-decoration: underline;
            color: orange;
        }
    }
}
.aside-recommend {
    width: 260px;
    margin-top: 20px;
    h4 {
        font-weight: 400;
        padding-bottom: 10px;
        border-bottom: 1px solid #ddd;
        margin-bottom: 10px;
    }
    img {
        display: block;
        width: 100%;
        cursor: pointer;
    }
}
</style>
