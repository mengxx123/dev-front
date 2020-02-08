<template>
    <my-page title="开发运维" :page="page">
        <div class="common-container">
            <app-list :data="groups" />
        </div>
    </my-page>
</template>

<script>
    /* eslint-disable */
    export default {
        data () {
            return {
                groups: [
                    {
                        name: '分类',
                        apps: [
                            {
                                name: '代码',
                                desc: '',
                                icon: '/static/img/code.svg',
                                to: 'xxx',
                                href: 'https://code.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: '网络',
                                desc: '',
                                icon: '/static/img/network.svg',
                                to: 'xxx',
                                href: 'https://network.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: 'SEO',
                                desc: '',
                                icon: '/static/img/seo.svg',
                                to: 'xxx',
                                href: 'https://seo.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: '编码解码',
                                desc: '',
                                icon: '/static/img/coding.svg',
                                to: 'xxx',
                                href: 'https://coding.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: '加密解密',
                                desc: '',
                                icon: '/static/img/encrypt.svg',
                                to: 'xxx',
                                href: 'https://crypto.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: '颜色',
                                desc: '',
                                icon: '/static/img/color.svg',
                                to: 'xxx',
                                href: 'https://color.yunser.com/',
                                target: '_blank'
                            }
                        ]
                    },
                    {
                        name: '基础',
                        apps: [
                            {
                                name: '正则表达式',
                                desc: '',
                                icon: '/static/img/regex.svg',
                                to: 'xxx',
                                href: 'https://regex.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: 'HTTP',
                                desc: '',
                                icon: '/static/img/http.svg',
                                to: 'xxx',
                                href: 'https://http.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: '查看网页代码',
                                desc: '',
                                icon: '/static/img/http.svg',
                                to: '/viewSource',
                            }
                        ]
                    },
                    {
                        name: '其他',
                        apps: [
                            {
                                name: '目录结构生成',
                                desc: '',
                                icon: '/static/img/directory.svg',
                                to: 'xxx',
                                href: 'https://directory.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: 'Crontab 表达式',
                                desc: '',
                                icon: '/static/img/crontab.svg',
                                to: 'xxx',
                                href: 'https://crontab.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: 'Linux 命令大全',
                                desc: '',
                                icon: '/static/img/build.svg',
                                to: 'xxx',
                                href: 'https://cmd.yunser.com/',
                                target: '_blank'
                            },
                            {
                                name: 'UUID 生成器',
                                desc: '',
                                icon: '/static/img/build.svg',
                                to: 'xxx',
                                href: 'https://util.yunser.com/uuid',
                                target: '_blank'
                            },
                            {
                                name: 'HDB3码',
                                desc: '',
                                icon: '/static/img/build.svg',
                                to: 'xxx',
                                href: 'https://hardware.yunser.com/messageCode',
                                target: '_blank'
                            },
                            {
                                name: '图（数据结构）',
                                desc: '',
                                icon: '/static/img/build.svg',
                                to: '/graph',
                            },
                        ]
                    }
                ],
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'search',
                            href: 'https://search.yunser.com?utm_source=dev',
                            target: '_blank',
                            title: '搜索'
                        },
                        {
                            type: 'icon',
                            icon: 'apps',
                            href: 'https://app.yunser.com?utm_source=dev',
                            target: '_blank',
                            title: '应用'
                        }
                    ]
                }
            }
        }
    }
</script>

<style scoped>
</style>
