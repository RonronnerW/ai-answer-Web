<template>
  <a-modal v-model:visible="visible" @cancel="closeModal" :footer="false">
    <template #title> 分享</template>
    <h4 style="margin-top: 0">复制分享链接</h4>
    <a-typography-paragraph copyable>{{ link }}</a-typography-paragraph>
    <h4>手机扫码查看</h4>
    <img :src="code" alt="" />
  </a-modal>
</template>
<script setup lang="ts">
import { defineProps, ref, withDefaults } from "vue";
import { defineExpose } from "vue";
import QRCode from "qrcode";

/**
 * 定义组件属性
 */
interface Props {
  title: string;
  link: string;
}

/**
 * 设置属性初始值
 */
const props = withDefaults(defineProps<Props>(), {
  title: () => "分享",
  link: () => "https://github.com/",
});
// 是否可见
const visible = ref(false);
// 要展示的二维码
const code = ref();

// 打开弹窗
const openModal = () => {
  visible.value = true;
};
// 关闭弹窗
const closeModal = () => {
  visible.value = false;
};
// 二维码生成
QRCode.toDataURL(props.link)
  .then((url: string) => {
    code.value = url;
  })
  .catch((err: string) => {
    console.error(err);
  });
// 暴露函数给父组件
defineExpose({
  openModal,
});
</script>

<style scoped></style>
