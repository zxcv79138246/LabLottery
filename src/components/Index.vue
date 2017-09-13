<template>
    <div id="index">
        <div class="header">
            <h1>抽籤系統</h1>
        </div>
        <div class="contain">
            <div class="members">
                <h2>成員</h2>
                <el-button class="member" :key="index" v-for="(member, index) in members">{{member.name}}</el-button>
            </div>
            <div class="button-box">
                <el-button type="info" class="draw-button" @click="clickDrawLots">抽籤</el-button>
            </div>
            <div class="progress">
                <el-progress type="circle" :percentage="percent" :status="progressStatus"></el-progress>
            </div>
            <div class="result">
                <h2>中獎</h2>
                <transition-group name="fade">
                    <el-button type="danger" class="member" :key="index" v-for="(name, index) in resultMember" v-show="percent >= (index+1) * (100/choiceSize)">{{name}}</el-button>
                </transition-group>
            </div>
        </div>
    </div>
</template>

<script>
import Config from '../../static/config.js'
export default {
    data() {
        return {
            members: Config.members,
            percent: 0,
            progressStatus: "",
            choiceSize: Config.choiceSize,
            resultMember: []
        }
    },
    methods: {
        clickDrawLots() {
            this.percent = 0
            this.progressStatus = ""
            var progress = setInterval(() => {
                if (this.percent < 100) {
                    this.percent++
                }
                else {
                    this.progressStatus = "success"
                    clearInterval(progress)
                }
            }, 35)
            this.randomNumber()
        },
        randomNumber() {
            this.resultMember = []
            var memberLength = this.members.length
            var totalMember = 0
            while (true) {
                var index = Math.floor((Math.random() * memberLength) + 0);
                var member = this.members[index]
                if (this.resultMember.indexOf(member.name) == -1) {
                    this.resultMember.push(member.name)
                    totalMember++
                }
                if (totalMember == 5) {
                    break
                }
            }
        }
    },
    created() {
    }
}
</script>

<style lang="scss">
#index {
    padding: 0 50px;
    display: flex;
    flex-direction: column;
    align-items: center;

    .contain {
        width: 100%;

        .members {

            .member {
                margin-bottom: 15px;
            }
        }

        .button-box {
            display: flex;
            justify-content: center;
            .draw-button {
                width: 300px;
                height: 70px;
            }
        }

        .progress {
            margin-top: 30px;
            display: flex;
            justify-content: center;
        }

        .result {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
    }

    .fade-enter-active,
    .fade-leave-active {
        transition: opacity .5s
    }
    .fade-enter,
    .fade-leave-to {
        opacity: 0
    }
}
</style>