<template>
  <div class="author-list-main">
    <div>
      <div>
        <VSearchForm
          slot="search"
          hint="请输入UP主名称，或者uid"
          @getSearchValue="getSearchValue"
        />
        <VCard style="margin-bottom:5px">
          <VTabs fixed-tabs>
            <VTab @click="sortChange(0)">
              粉丝总数
            </VTab>
            <VTab @click="sortChange(1)">
              播放总量
            </VTab>
            <VTab @click="sortChange(2)">
              专栏阅读
            </VTab>
          </VTabs>
        </VCard>
        <VSlideYTransition group>
          <VCard
            v-for="eachAuthor in authorList.content"
            :key="eachAuthor.mid"
            class="author-cards"
            ripple
            :to="'/author/' + eachAuthor.mid"
          >
            <div style="padding:5px;display:flex">
              <div>
                <VImg
                  style="border-radius:40px;width:80px;height:80px"
                  :src="eachAuthor.face.replace('http:', '')"
                  :lazy-src="eachAuthor.face.replace('http:', '')"
                />
              </div>
              <div style="margin-left:10px;width:100%">
                <div class="font-weight-bold author-title">
                  {{ eachAuthor.name }}
                  <SexIcon :sex="eachAuthor.sex" />
                </div>
                <div
                  v-if="eachAuthor.official !== ''"
                  class="caption  author-info"
                >
                  <VIcon
                    color="#FBC02D"
                    small
                  >
                    mdi-flash </VIcon>{{ eachAuthor.official }}
                </div>
              </div>
              <ObserveStatus
                class="observe-status"
                :object="eachAuthor"
              />
            </div>
          </VCard>
        </VSlideYTransition>
        <VBtn
          block
          outline
          color="blue darken-2"
          :disabled="nextBtnDisabled"
          @click.stop="next"
        >{{ nextBtnText }}</VBtn>
      </div>
    </div>
  </div>
</template>

<script>
import VSearchForm from "../common/VSearchForm.vue";
import ObserveStatus from "../common/ObserveStatus.vue";
import SexIcon from "../common/SexIcon.vue";
export default {
  name: "AuthorList",
  components: {
    VSearchForm,
    SexIcon,
    ObserveStatus
  },
  data() {
    return {
      sort: 0,
      authorList: [],
      currentApiurl: String(),
      currentPage: 0,
      text: String(),
      nextBtnText: "请给我更多...",
      nextBtnDisabled: false,
      requestUrl: String()
    };
  },
  watch: {
    text: function() {
      this.currentPage = 0;
      this.axios
        .get(
          this.currentApiurl +
            "?page=" +
            this.currentPage +
            "&text=" +
            this.text +
            "&sort=" +
            this.sort
        )
        .then(response => {
          this.authorList.content = response.data.content;
        });
    },
    currentPage: function changePage(page) {
      this.axios
        .get(
          `${this.currentApiurl}?page=${page}&text=${this.text}&sort=${
            this.sort
          }`
        )
        .then(response => {
          // 判断是否为最后一页
          if (response.data.last) {
            this.nextBtnText = "没有更多了";
            this.nextBtnDisabled = true;
          }
          response.data.content.forEach(e => {
            this.authorList.content.push(e);
          });
        });
    }
  },
  created() {
    this.currentApiurl = "/author";
    this.axios.get(this.currentApiurl).then(response => {
      this.refreshList(response);
    });
  },
  methods: {
    refreshList(response) {
      this.authorList = response.data;
      // 判断是否为最后一页
      if (response.data.last) {
        this.nextBtnText = "没有更多了";
        this.nextBtnDisabled = true;
      }
    },
    onScroll() {
      var scrollTop =
        document.documentElement.scrollTop || document.body.scrollTop;
      var windowHeight =
        document.documentElement.clientHeight || document.body.clientHeight;
      var scrollHeight =
        document.documentElement.scrollHeight || document.body.scrollHeight;
      if (scrollTop + windowHeight == scrollHeight) {
        // this.currentPage += 1;
      }
    },
    next() {
      this.currentPage += 1;
    },
    getSearchValue(value) {
      this.text = value;
    },
    handleChoosed(index, row) {
      this.$router.push({
        path: "/author/" + row.mid + "/author/" + row.aid
      });
    },
    sortChange(sort) {
      this.sort = sort;
      this.currentPage = 0;
      this.axios
        .get(
          `${this.currentApiurl}?page=${this.currentPage}&text=${
            this.text
          }&sort=${this.sort}`
        )
        .then(response => {
          this.refreshList(response);
        });
    }
  }
};
</script>

<style>
.face {
  position: relative;
  height: 60%;
  width: 60%;
  align-content: center;
  border-radius: 4px;
}

.el-table td {
  padding: 2px 0;
}

p {
  position: absolute;
  bottom: 0px;
  margin-bottom: 5px;
}

.author-cards {
  margin: 10px 2px;
  height: 90px;
  border-radius: 5px;
}

.author-title {
  max-width: 50vw;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.observe-status {
  position: absolute;
  bottom: 5px;
  right: 5px;
}

.author-info {
  margin-top: 5px;
}
</style>
