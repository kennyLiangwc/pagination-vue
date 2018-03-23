<template>
    <div>
        <div class="pagination">
            <ul>
                <li :class="{'disabled': myCurrent == 1}" @click="setCurrent(1)">&lt;&lt;</li>
                <li :class="{'disabled': myCurrent == page}" @click="setCurrent(myCurrent - 1)">&lt;</li>
                <li v-for="group in grouplist" :class="{'sel': myCurrent == group.val}" @click="setCurrent(group.val)"
                    v-if="group.text >0">{{group.text}}
                    <!--<span :class="{HeartAnimation:myCurrent == group.val,'like-active':myCurrent == group.val}"></span>-->
                </li>
                <li :class="{'disabled': myCurrent ==page}" @click="setCurrent(myCurrent + 1)">&gt;</li>
                <li :class="{'disabled': myCurrent == page}" @click="setCurrent(page)">&gt;&gt;</li>
            </ul>
            <p>共{{total}}条，{{page}}页，每页{{pageSize}}条</p>
        </div>
    </div>
</template>
<script>
    /**
     * [pagination      分页组件]
     *@param {Number} total [数据总条数]
     *@param {Number} pageSize  [每页显示条数 default:10]
     *@param {Number} pageNumber   [当前页码 default:1]
     *@param {Number} pagegroup  [分页条数 default:5]
     *@param {Event} pagechange  [页码改动时   emit]
     *@return {[type]}  [description]
     */
    export default {
        data () {
            return {
                myCurrent: this.pageNumber,
                value: this.pageSize
            }
        },

        props: {
            total: {        //数据总条数
                type: Number,
                default: 0
            },
            pageNumber: {
                type: Number,
                default: 1
            },
            pageSize: {
                type: Number,
                default: 10
            },
            pagegroup: {        //分页条数
                type: Number,
                default: 10,
                coerce (v) {
                    v = v > 0 ? v : 5;
                    return v % 2 === 1 ? v : v + 1;
                }
            }
        },
        watch: {
            pageNumber(val, oldValue){
                this.myCurrent = val
            }
        },
        computed: {
            page () {   //总页数
                return Math.ceil(this.total / this.value);
            },
            grouplist () {      //获取分页页码
                var len = this.page, temp = [], list = [], count = Math.floor(this.pagegroup / 2), center = this.myCurrent;

                if (len <= this.pagegroup) {
                    while (len--) {
                        temp.push({text: this.page - len, val: this.page - len})
                    }
                    return temp;
                }
                while (len--) {
                    temp.push(this.page - len)
                }
                var idx = temp.indexOf(center);
                ( idx < count ) && ( center = center + count - idx );
                (this.myCurrent > this.page - count ) && (center = this.page - count + 1);
                temp = temp.splice(center - count - 1, this.pagegroup);
                do {
                    var t = temp.shift();
                    list.push({
                        text: t,
                        val: t
                    })
                } while (temp.length);
                if (this.page > this.pagegroup) {
                    (this.myCurrent > count ) && list.unshift({text: list[0].val - 1, val: list[0].val - 1});
                    (this.myCurrent < this.page - count + 1) && list.push({
                        text: list[list.length - 1].val + 1,
                        val: list[list.length - 1].val + 1
                    });
                }
                return list;
            }
        },
        methods: {
            setCurrent (idx) {
                if (this.myCurrent != idx && idx > 0 && idx < this.page + 1) {
                    this.myCurrent = idx;
                    const params = Object.assign({}, {
                        pageNumber: this.myCurrent,
                        pageSize: Number(this.value)
                    });
                    this.$emit('pagechange', params)
                }
            }
        }
    }
</script>
<style scoped>
    .liSel {
        position: relative;
    }
    .pagination {
        padding:10px 10px 10px 0;
        line-height:28px;
    }
    .pagination ul {
        list-style:none;
        float:right;
    }
    .pagination ul li {
        display:inline-block;
        min-width:28px;
        height:28px;
        line-height:28px;
        text-align:center;
        font-size:16px;
        margin-left:4px;
        cursor:pointer;
    }
    .pagination ul li.sel, .pagination ul li:hover {
        background:#44b549;
        color:#fff;
    }
</style>