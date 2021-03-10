<template>
    <div class="impression-index">
        <div class="navbar">
            <div class="title">印象绵阳：</div>
            <ul class="nav">
                <li
                    v-for="item in navList"
                    :key="item.key"
                    :class="nav_key === item.key ? 'active' : ''"
                    @click="handleChangeKey(item.key)"
                >
                    {{ item.name }}
                </li>
            </ul>
        </div>
        <div class="content">
            <generalize v-if="nav_key === 'generalize'" />
            <scenic v-if="nav_key === 'scenic'" />
            <position v-if="nav_key === 'position'" />
        </div>
    </div>
</template>

<script>
import Generalize from '@/components/impression/generalize'
import Scenic from '@/components/impression/scenic'
import Position from '@/components/impression/position'

export default {
    name: 'impression',
    components: {
        Generalize,
        Scenic,
        Position
    },
    data() {
        return {
            nav_key: 'generalize',
            navList: [
                {
                    name: '绵阳概括',
                    key: 'generalize'
                },
                {
                    name: '绵阳景区',
                    key: 'scenic'
                },
                {
                    name: '绵阳定位',
                    key: 'position'
                }
            ]
        }
    },
    methods: {
        handleChangeKey(key) {
            this.nav_key = key
        }
    }
}
</script>

<style lang="less" scoped>
.impression-index {
    width: 1000px;
    margin: 30px auto;
    min-height: 80vh;
    .navbar {
        width: 100%;
        display: flex;
        align-items: center;
        .title {
            border-radius: 5px;
            background: #005ba1;
            color: white;
            font-size: 14px;
            font-weight: bold;
            padding: 5px 10px;
        }
        .nav {
            display: flex;
            cursor: pointer;
            li {
                color: #777777;
                font-size: 14px;
                margin-left: 20px;
            }
            li:hover {
                color: #005ba1;
            }
            .active {
                color: #005ba1;
            }
        }
    }
}
</style>
