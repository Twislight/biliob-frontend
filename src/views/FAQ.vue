<template>
  <MainLayout reverse>
    <div slot="aside-cards">
      <VCard class="mb-2">
        <VCardTitle class="title blue--text text--darken-3 font-weight-black">
          <VIcon color="blue darken-3">mdi-file-document-box-outline</VIcon>目录
        </VCardTitle>
      </VCard>
      <VCard class="mb-2">
        <VCardText>
          <ol
            v-for="(eachCatalog,catalogIndex) in questionCards"
            :key="catalogIndex"
            class="subheading mb-2"
          >
            <a :href="`#${eachCatalog.catalogName}`">{{eachCatalog.catalogName}}</a>
            <li
              v-for="(eachQuestion, questionIndex) in eachCatalog.catalogQuestions"
              :id="eachQuestion.question"
              :key="questionIndex"
              class="body-1"
            ><a :href="`#${eachQuestion.question}`">{{eachQuestion.question}}</a></li>
            <VDivider></VDivider>
          </ol>
        </VCardText>
      </VCard>
    </div>
    <div slot="main-cards">
      <VCard class="mb-2">
        <VCardTitle class="title font-weight-black blue--text text--darken-3">
          <VIcon color="blue darken-3"> mdi-comment-question-outline </VIcon>常见问题解答
        </VCardTitle>
      </VCard>
      <VCard
        v-for="(eachCatalog,catalogIndex) in questionCards"
        :key="catalogIndex"
        class="mb-1"
      >
        <VCardTitle class="blue--text text--darken-3 subheading font-weight-black">
          <li :id="eachCatalog.catalogName">
            {{eachCatalog.catalogName}}
          </li>
        </VCardTitle>
        <VCardText
          v-for="(eachQuestion, questionIndex) in eachCatalog.catalogQuestions"
          :key="questionIndex"
        >
          <div
            :id="eachQuestion.question"
            class="subheading font-weight-regular blue--text text--darken-3"
          >
            问题{{ questionIndex + 1 }}：{{ eachQuestion.question }}
          </div>
          <div
            v-for="(eachParagraph, pIndex) in eachQuestion.answer"
            :key="pIndex"
            class="body-1"
          >
            {{ eachParagraph }}
          </div>
        </VCardText>
      </VCard>
    </div>
  </MainLayout>
</template>
<script>
import MainLayout from "../components/common/MainLayout.vue";
export default {
  components: { MainLayout },
  data() {
    return {
      questionCards: [
        {
          catalogName: "BiliOB观测者哲学三问",
          catalogQuestions: [
            {
              question: "BiliOB观测者是什么？",
              answer: [
                "BiliOB观测者是由Jannchie见齐创建并维护的一个Web应用程序。",
                "BiliOB观测者是一个观测Bilibili上的UP主与其视频的各个指标的数据及其变化趋势的第三方网站。",
                "在这里，你可以自由添加UP主或视频观测，了解到最新的榜单和粉丝变动情报。"
              ]
            },
            {
              question: "BiliOB从哪里来？",
              answer: [
                "BiliOB观测者由Jannchie见齐创建并维护。",
                "BiliOB观测者的所有数据来自于B站官方正在使用的API。",
                "BiliOB观测者的资金支持主要来自于B站创作激励计划、高能联盟计划、以及直播礼物和充电。"
              ]
            },
            {
              question: "BiliOB到哪里去？",
              answer: [
                "BiliOB观测者通过搜集并观测B站的UP主、视频等数据，给UP主作为参考，了解自己作品的受欢迎程度。从而帮助作者创作更加优秀的作品。",
                "BiliOB观测者能够持续观测B站数据，记录B站的发展历程以及各个时期的热点情况。",
                "另一方面，通过观测这些数据，能够了解到B站的最新动态。这些数据本身就非常具有观赏性。"
              ]
            }
          ]
        },
        {
          catalogName: "积分相关问题",
          catalogQuestions: [
            {
              question: "如何获得积分和经验？",
              answer: [
                "目前，每日签到可以获得10点积分和经验值。",
                "消耗积分可以增加等量的经验。"
              ]
            },
            {
              question: "签到间隔时间是多长？",
              answer: ["已注册的用户，每隔8小时就可以签到一次。"]
            },
            {
              question: "积分可以用来做什么？",
              answer: [
                "积分可以用于强制追踪up主、增加追踪频率、进行视频弹幕分析等功能。"
              ]
            }
          ]
        }
      ]
    };
  },
  computed: {
    brightness() {
      if (this.$store.state.dark) {
        return "lighten";
      } else {
        return "darken";
      }
    }
  }
};
</script>
<style scoped>
.question-card {
  margin-bottom: 10px;
}
a {
  color: #424242;
  text-decoration: none;
}
.theme--dark a {
  color: #fff;
  text-decoration: none;
}
.theme--dark a:hover {
  color: #3c89e0;
  text-decoration: none;
}
a:hover {
  color: #1565c0;
}
</style>
