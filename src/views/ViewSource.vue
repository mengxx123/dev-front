<template>
    <my-page title="查看网页代码" :page="page">
        <div class="common-container">
            <ui-text-field v-model="form.url" label="链接" />
            <br>
            <div class="btns">
                <ui-raised-button label="查看源代码" primary @click="view" />
            </div>
            <br>
            <textarea class="textarea" v-model="result" placeholder="请求结果" rows="16" v-if="result"></textarea>

        </div>
    </my-page>
</template>

<script>
    /* eslint-disable */
    export default {
        data () {
            return {
                form: {
                    url: '',
                },
                result: '',
                scripts: [],
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'help',
                            href: 'https://project.yunser.com/products/2e56d9203f3d11ea9836c5fd04470d6c',
                            target: '_blank',
                            title: '应用'
                        }
                    ]
                }
            }
        },
        mounted() {
            // this.debug()
        },
        methods: {
            debug() {
                this.form.url = 'https://app.yunser.com/'
            },
            view() {
                function parseHtml(html, url) {
                    // script
                    let match = html.match(/<script[\d\D]+?>[^<]+?/g)
                    console.log('match', match)
                    let scripts = []
                    let protocol = url.includes('https://') ? 'https:' : 'http:'
                    if (match) {
                        for (let item of match) {
                            if (item.includes('src=')) {
                                let relativeUrl = item.match(/src="([\d\D]+?)"/)[1]
                                let absoluteUrl = relativeUrl
                                if (relativeUrl.match(/^\/\//)) {
                                    absoluteUrl = protocol + '' + relativeUrl
                                } else if (!relativeUrl.includes('http')) {
                                    absoluteUrl = url + '' + relativeUrl
                                } else {
                                // absoluteUrl = '?'

                                }
                                scripts.push(absoluteUrl)
                            }
                        }
                    }
                    console.log('scripts', scripts)
                    
                    // img
                    match = html.match(/<img[\d\D]+?>[^<]+?/g)
                    console.log('match', match)
                }
                    
                let url = this.form.url
                if (!url) {
                    this.$message({
                        type: 'danger',
                        text: '请输入地址'
                    })
                    return
                }
                this.$http.get(`/http/source?url=${encodeURIComponent(url)}`).then(
                    res => {
                        console.log('res', res)
                        this.result = res.data
                        let html = res.data
                        this.scripts = parseHtml(html, url)
                        console.log('this.scripts', this.scripts)
                        // let data = response.data
                        // this.times = data.data
                        // this.loading = false
                    },
                    response => {
                        this.$message({
                            type: 'danger',
                            text: '获取失败'
                        })
                        // console.log(response)
                        // this.loading = false
                    })
            },
        }
    }
</script>

<style scoped>
.btns {
    margin-bottom: 16px;
}
textarea.textarea {
    height: auto;
}
.textarea {
    display: block;
    width: 100%;
    /* max-width: 400px; */
    height: 400px;
    padding: 6px 12px;
    font-size: 14px;
    line-height: 1.42;
    color: #55595c;
    background-color: #fff;
    background-image: none;
    border: 1px solid #ccc;
    border-radius: 2px;
    outline: none;
}
</style>
