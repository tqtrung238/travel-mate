<template>
  <div class="block">
    <el-timeline>
      <el-timeline-item v-for="(item,index) of timeline" :key="index" :timestamp="item.timestamp" placement="top">
        <el-card>
          <h4>{{ item.title }}</h4>
          <p>{{ item.content }}</p>
        </el-card>
      </el-timeline-item>
    </el-timeline>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timeline: [
        {
          timestamp: '2019/4/20',
          title: 'Thác Bạc',
          content: 'Đã tới Thác Bạc- SaPa 2019/4/20 20:46'
        },
        {
          timestamp: '2019/4/21',
          title: 'Bảo tàng Hà Nội',
          content: '2019/4/21 20:46'
        },
        {
          timestamp: '2019/4/22',
          title: 'Thành nhà Hồ',
          content: '2019/4/22 20:46'
        },
        {
          timestamp: '2019/4/23',
          title: 'Hoàng thành Thăng Long',
          content: '2019/4/23 20:46'
        }
      ]
    }
  }
}
</script>
