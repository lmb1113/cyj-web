<template>
    <div class="item">
        <t-row>
            <div class="logo-div">
                <span style="font-size: 30px;font-weight: bold;">穿云箭</span>
            </div>
        </t-row>
        <t-row style="margin-top: 10px;">
            <span>拨开云雾见青天</span>
        </t-row>
        <t-row style="margin-top: 100px;">
            <span>内网地址</span>
        </t-row>
        <t-row>
            <t-col :span="24">
                <div>
                    <t-input size="large" @input="handleInput" v-model="localhost"
                        placeholder="请输入需要穿透的本地服务地址,例如(127.0.0.1:8081)"></t-input>
                </div>
            </t-col>
        </t-row>
        <t-row style="margin-top: 10px;">
            <span>映射协议</span>
        </t-row>
        <t-row>
            <t-col :span="24">
                <div>
                    <t-radio-group variant="default-filled" default-value="4" v-model="proxyType">
                        <t-radio-button v-for="item in proxyTypeList" :value="item">{{ item }}</t-radio-button>
                    </t-radio-group>
                </div>
            </t-col>
        </t-row>
        <t-row v-if="remoteAddr != ''" style="margin-top: 20px;">
            <span>外网地址</span>
        </t-row>
        <t-row v-if="remoteAddr != ''">
            <t-input size="large" status="success" v-model="remoteAddr">
                <template #suffixIcon>
                    <copy-icon @click="copy(remoteAddr)" />
                </template>
            </t-input>
        </t-row>
        <t-row>
            <div class="start-div" v-if="recording">
                <t-button size="large" theme="danger" style="width: 100%;margin-top: 20px;"
                    @click="close()">停止穿透</t-button>
            </div>
            <div class="start-div" v-else>
                <t-button size="large" theme="primary" style="width: 100%;margin-top: 20px;"
                    @click="startRecording">开始穿透</t-button>
            </div>
        </t-row>
        <t-row style=" position: absolute;bottom: 2%; left: 0;right: 0;">
            <div class="copyright-div">
                <div>
                    <t-link style="margin: 5px;" theme="primary" @click="dialogVisible = true"
                        hover="color">使用指南</t-link>
                    <t-link style="margin: 5px;" theme="primary" hover="color"
                        @click="gotoBrowser('https://c.0a0a.cn')">软件官网</t-link>
                    <t-link style="margin: 5px;" theme="primary" target="_blank"
                        @click="gotoBrowser('https://qm.qq.com/q/lhm1yvQQN4')" hover="color">交流Q群</t-link>
                </div>
                <p style="font-size: 10px;">©穿云箭 2018-2025 使用协议</p>
                <p style="font-size: 10px;">version 1.0.6</p>
            </div>
        </t-row>
        <t-dialog title="常见问题指南" v-model:visible="dialogVisible" width="90%"
            :on-confirm="() => (dialogVisible = false)">
            <div class="qa">
                <span class="qa-title">1.问:什么是穿云箭？</span>
                <span class="qa-context">答:为爱发电，内网穿透工具，适用于本地端口的外网访问</span>
            </div>
            <div class="qa">
                <span class="qa-title">2.问:可以翻墙吗？</span>
                <span class="qa-context">答:不可以，并非VPN，只是一种内网穿透工具</span>
            </div>
            <div class="qa">
                <span class="qa-title">3.问:有使用案例么</span>
                <div class="qa-context">
                    <div class="qa-context-div">
                        <div style="background-color:#dee3e9;padding: 5px;border-radius: 5px;">
                            <t-tag theme="success">联调篇</t-tag>
                            来自深圳的后端小明同学开了一个java服务，使用的是8080端口，此时远在北京的前端小李同学需要访问这个服务进行调试，但是发布到测试环境又不方便调试，此时小明就可以用本工具，填写127.0.0.1:8080，点击开启穿透，将地址发给小李同学访问
                        </div>
                    </div>
                    <div class="qa-context-div">
                        <div style="background-color:#dee3e9;padding: 5px;border-radius: 5px;">
                            <t-tag theme="primary">对接篇</t-tag>
                            真实案例，某信/某宝的支付对接，需要回调支付成功接口，此时如果先写好代码再打包到测试环境，只能依赖日志进行调试，但是有了此工具，可以使用穿透将支付成功的请求直接访问到本地进行断点调试，开发效率拉满！！！
                        </div>
                    </div>
                    <div class="qa-context-div">
                        <div style="background-color:#dee3e9;padding: 5px;border-radius: 5px;">
                            <t-tag theme="danger">其他篇</t-tag>
                            更多功能，期待您的挖掘
                        </div>
                    </div>
                </div>
            </div>

        </t-dialog>
    </div>
</template>
<script setup>
import { ref, reactive } from 'vue'
import { MessagePlugin } from 'tdesign-vue-next'
import useClipboard from 'vue-clipboard3'
const localhost = ref("127.0.0.1:8080")
const remoteAddr = ref("")
const proxyType = ref("http")
const proxyTypeList = ref(["http", "https"])
const recording = ref(false)
const dialogVisible = ref(false)
import {
    CopyIcon,
} from 'tdesign-icons-vue-next';

const handleInput = (value) => {
    localhost.value = value.replace(/[^0-9:.]/g, '') // 更新绑定的值  
}
const startRecording = () => {
    // recording.value = true
    MessagePlugin.success("请下载后使用")
}
const close = () => {
    recording.value = false
    remoteAddr.value = ""
    MessagePlugin.success("停止成功")

}

const { toClipboard } = useClipboard()
const copy = async (text) => {
    try {
        await toClipboard(text) // 实现复制
        MessagePlugin.success('复制成功')
    } catch (e) {
        console.error(e)
    }
}

</script>
<style scoped>
* {
    user-select: none;
}

.t-row {
    display: block;
}

.t-link {
    display: inline;
}


.logo-div {
    display: flex;
    align-items: center;
}

.start-div {
    display: flex;
    align-items: center;
    justify-content: center;
}

.transcribe-div {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100px;
    border-radius: 10px;
    border: 2px solid rgb(207, 207, 207);
}

.transcribe-div:hover {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100px;
    border: 2px;
    border-radius: 10px;
    border: 2px solid rgb(75, 119, 238);
}


.transcribe-div-select {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100px;
    border: 2px;
    border-radius: 10px;
    border: 2px solid rgb(75, 119, 238);
}

.setting-div {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.copyright-div {
    position: relative;
    display: flex;
    justify-content: center;
    text-align: center;
    flex-direction: column;
}

.qa-title {
    font-weight: bold;
    size: 16px;
}

.qa-context {
    size: 16px;
}

.qa {
    display: flex;
    flex-direction: column;
}

.qa-context-div {
    margin-top: 5px;
}

.item {
    background-image: linear-gradient(-225deg, #E3FDF5, #FFE6FA 100%);
    width: 480px;
    height: 700px;
    padding: 20px;
    border-radius: 10px;
}
</style>