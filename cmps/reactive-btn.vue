<template>
  <div class="btn-container">
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="cell"></div>
    <div class="content">
      <button class="action-btn">
        <span><img src="../assets/svg/search.svg" style="width:14px" alt="" />{{ text }}</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
    },
  },
  created() {
    
  },
};
</script>

<style lang="scss" scoped>
.btn-container {
  grid-row: 1/-1;
  grid-column: 2;
  position: relative;
  width: 100%;
  height: 48px;
  padding: 0 0;
  margin: 0 0;
  cursor: pointer;
  border: none;
  display: grid;
  grid-template: repeat(10, 1fr) / repeat(10, 1fr);

  @for $i from 0 to 10 {
    .cell:nth-child(10n + #{$i + 1}):hover ~ .content {
      --positionX: #{$i};
    }

    .cell:nth-child(n + #{10 * $i + 1}):nth-child(-n + #{10 * ($i + 1)}):hover
      ~ .content {
      --positionY: #{$i};
    }
  }
}

.cell {
  width: 100%;
  height: 100%;
  z-index: 1;
}

.content {
  --positionX: 0;
  --positionY: 5;

  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  border-radius: 8px;

  background-image: radial-gradient(
    circle at calc((0 + var(--positionX, 0)) * 10%)
      calc((0 + var(--positionY, 0)) * 5%),
    rgb(241, 37, 75) 0%,
    rgb(230, 30, 77) 27.5%,
    rgb(227, 28, 95) 40%,
    rgb(215, 4, 102) 57.5%,
    rgb(189, 30, 89) 75%,
    rgb(189, 30, 89) 100%
  );
}

.action-btn {
  padding: 0 0;
  margin: 0 0;
  width: 100%;
  height: 100%;
  background-color: transparent;
  border: none;
}

span {
  padding: 0 0;
  margin: 0 0;
  color: white;
  font-size: 16px;
  line-height: 20px;
  font-weight: 600;
  font-family: sans-serif;
}
</style>
