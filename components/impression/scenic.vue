<template>
    <div class="scenic">
        <el-card class="card" v-loading="loading">
            <div v-for="item in scenicList.data" :key="item.id" class="scenic-content mt-10">
                <div class="cont-left">
                    <img :src="item.scenic_img" class="banner" />
                </div>
                <div class="cont-right">
                    <h2 class="fw-bold">{{ item.scenic_name }}</h2>
                    <p class="gray fs-xs mt-10">地址： {{ item.scenic_address }}</p>
                    <p class="gray fs-xs mt-10">联系电话： {{ item.scenic_phone }}</p>
                </div>
            </div>

            <!-- <el-empty
                v-if="scenicList.length === 0"
                description="肥肠抱歉， 暂时没有数据......"
            ></el-empty> -->
        </el-card>

        <div class="page">
            <el-pagination
                background
                layout="prev, pager, next"
                :total="scenicList.count"
                @current-change="getScenic"
            >
            </el-pagination>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Scenic',
    mounted() {
        this.getScenic()
    },
    data() {
        return {
            scenicList: {},
            loading: false,
            page: 0
        }
    },
    methods: {
        async getScenic(page) {
            this.page = page
            this.loading = true
            const res = await this.$axios({
                url: '/travel/scenic',
                params: {
                    page
                }
            })
            this.scenicList = res.data
            this.loading = false
        }
    }
}
</script>

<style lang="less" scoped>
.scenic {
    .card {
        margin-top: 20px;
    }
    &-header {
        width: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 10px 0;
    }
    &-content {
        border-radius: 10px;
        display: flex;
        padding: 10px;
        transition: all 300ms;
        .cont-left {
            flex-basis: 40%;
            .banner {
                border-radius: 10px;
                flex: 0 0 auto;
                width: 280px;
                height: 200px;
            }
        }
        .cont-right {
            position: relative;
            flex: 1;
            p {
                margin-top: 10px;
            }
        }
    }
    &-content:hover {
        cursor: pointer;
        background: #eeeeee;
    }
    .page {
        margin: 20px 0;
    }
}
</style>
