<template>
  <VCard class="aside-card">
    <VCardTitle
      style="padding-bottom:0px"
      column
      fill-height
    >
      <h4>{{ title }}</h4>
    </VCardTitle>
    <VCardText
      class="video-list"
      column
    />
    <VList two-line>
      <template v-for="eachVideo in authorTopVideo.content">
        <VDivider :key="eachVideo.title" />
        <VListTile
          :key="eachVideo.aid"
          style="width: 100%"
          :to="'/author/'+eachVideo.mid+'/video/'+eachVideo.aid"
          ripple
        >
          <VListTileAvatar
            :key="eachVideo.title"
            tile
          >
            <VImg :src="eachVideo.pic" />
          </VListTileAvatar>
          <VListTileContent>
            <VListTileTitle>
              {{ eachVideo.title }}
            </VListTileTitle>
            <VListTileSubTitle class="caption subtext">
              分区：{{ eachVideo.channel }} 投稿时间：{{ dateToStr(eachVideo.datetime) }}
            </VListTileSubTitle>
          </VListTileContent>
        </VListTile>
      </template>
    </VList>
  </VCard>
</template>
<script>
var format = require("date-fns/format");
var { convertDateToUTC } = require("../../charts/util/convertDateToUTC");
export default {
  props: {
    title: String(),
    authorTopVideo: Object()
  },
  watch: {
    authorTopVideo: function(val) {
      this.authorTopVideo = val;
    }
  },
  methods: {
    dateToStr(d) {
      return format(
        convertDateToUTC(new Date(d.replace("+0000", ""))),
        "YYYY-MM-DD HH:mm"
      );
    }
  }
};
</script>
<style>
.link {
  text-decoration: none;
}

.aside-card {
  margin-bottom: 5px;
}

.video-list {
  padding-bottom: 0px;
  padding-top: 0px;
}
</style>
