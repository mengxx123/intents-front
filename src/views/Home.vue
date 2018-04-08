<template>
    <my-page title="首页">
        a.yunser.com
        <div id="tip"></div>
        <textarea id="data"></textarea>
        <button id="send">完成</button>
    </my-page>
</template>

<script>
    /* eslint-disable */
    export default {
        data () {
            return {
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'help',
                            to: '/help'
                        }
                    ]
                }
            }
        },
        mounted() {
            this.init()
        },
        methods: {
            init() {
                let actionName = ``
                function getActionName(name) {
                    let arr = name.split('/')
                    return arr[arr.length - 1]
                }
                let origin
                window.addEventListener('message',function(event) {
                    console.log('on message')
                    console.log(event)
                    // 通过origin属性判断消息来源地址
                    origin = event.origin
                    if (event.origin === 'http://a.com') {
                        console.log(event.data);
                        console.log(event.source);
                    }
                    let intent = event.data.action

                    console.log(event.data.action)
                    let actionName = getActionName(intent.action)
                    console.log('actionName', actionName)
                    if (intent.type !== 'text/plain') {
                        document.getElementById('tip').innerText = '不支持的类型'
                        return
                    }
                    document.getElementById('data').value = intent.data
                },false);
                document.getElementById('send').addEventListener("click", function() {
                    console.log('send message')
                    let data = document.getElementById('data').value
                    window.opener.postMessage(data, origin)
                    window.close()
                }, false);

                window.opener.postMessage('data to send222', origin)
            }
        }
    }
</script>

<style scoped>
</style>
