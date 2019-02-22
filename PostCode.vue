<template>
  <div>
    <div
      ref="searchWindow"
      :style="searchWindow"
      style="border:1px solid;width:500px;margin:5px 0;position:relative"
    >
      <img
        src="//t1.daumcdn.net/postcode/resource/images/close.png"
        id="btnFoldWrap"
        style="cursor:pointer;position:absolute;right:0px;top:-1px;z-index:1"
        @click="searchWindow.display = 'none'"
        alt="close"
      >
    </div>
    <input type="text" placeholder="우편번호" v-model="postcode">
    <input type="button" value="우편번호 찾기" @click="execDaumPostcode">
    <br>
    <input type="text" v-model="address" placeholder="주소">
    <br>
    <input type="text" v-model="extraAddress" ref="extraAddress" placeholder="상세주소">
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchWindow: {
        display: 'none',
        height: '300px',
      },
      postcode: '',
      address: '',
      extraAddress: '',
    };
  },
  methods: {
    execDaumPostcode() {
      const currentScroll = Math.max(
        document.body.scrollTop,
        document.documentElement.scrollTop,
      );

      // eslint-disable-next-line
      new daum.Postcode({
        onComplete: (data) => {
          if (data.userSelectedType === 'R') {
            this.address = data.roadAddress;
          } else {
            this.address = data.jibunAddress;
          }

          if (data.userSelectedType === 'R') {
            if (data.bname !== '' && /[동|로|가]$/g.test(data.bname)) {
              this.extraAddress += data.bname;
            }

            if (data.buildingName !== '' && data.apartment === 'Y') {
              this.extraAddress +=
                this.extraAddress !== ''
                  ? `, ${data.buildingName}`
                  : data.buildingName;
            }

            if (this.extraAddress !== '') {
              this.extraAddress = ` (${this.extraAddress})`;
            }
          } else {
            this.extraAddress = '';
          }

          this.postcode = data.zonecode;

          this.$refs.extraAddress.focus();

          this.searchWindow.display = 'none';
          document.body.scrollTop = currentScroll;
        },
        onResize: (size) => {
          this.searchWindow.height = `${size.height}px`;
        },
        width: '100%',
        height: '100%',
      }).embed(this.$refs.searchWindow);

      this.searchWindow.display = 'block';
    },
  },
};
</script>
