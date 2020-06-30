<template>
  <header>
    <div class="header-overlay"></div>
    <h1>Univers Cinématographique Marvel</h1>
    <button id="scrollBtn" class="btn-scroll">
      <a href="#timeline">
        <img
          src="../assets/arrow.png"
          alt="Flèches qui permettent de descendre directement à la timeline"
        />
      </a>
    </button>
  </header>
</template>

<script>
export default {
  name: "Header",
  methods: {
    size() {
      let xMax =
        window.innerWidth ||
        document.documentElement.clientWidth ||
        document.body.clientWidth;
      let yMax =
        window.innerHeight ||
        document.documentElement.clientHeight ||
        document.body.clientHeight;

      this.resizeHeader(xMax, yMax);
    },
    resizeHeader(x, y) {
      const header = document.querySelector("header");
      const scrollBtn = document.getElementById("scrollBtn");

      if (x >= 1024) {
        header.style.height = y + "px";
        scrollBtn.style.display = "block";
      } else {
        header.style.height = y / 4 + "px";
        scrollBtn.style.display = "none";
      }
    },
    onResize(event) {
      this.resizeHeader(event.target.innerWidth, event.target.innerHeight);
    }
  },

  mounted() {
    // Register an event listener when the Vue component is ready
    window.addEventListener("resize", this.onResize);
    this.size();
  },

  beforeDestroy() {
    // Unregister the event listener before destroying this Vue instance
    window.removeEventListener("resize", this.onResize);
  }
};
</script>

<style lang="scss">
header {
  position: relative;

  background-color: rgba(0, 0, 0, 0.5);
  background-image: url("../assets/wall.webp");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;

  h1 {
    position: relative;
    top: 50%;
    transform: translateY(-50%);

    color: #ffffff;

    font-size: 28px;
    font-style: normal;
    font-weight: bold;
    line-height: 28px;
    text-align: center;
  }

  .header-overlay {
    background-color: rgba(0, 0, 0, 0.5);
    position: absolute;
    top: 0;
    left: 0;

    width: 100%;
    height: 100%;
  }

  .btn-scroll {
    position: absolute;
    top: 90%;
    left: 50%;
    transform: translateX(-50%);
    display: none;
    background: transparent;
    border: none;
    cursor: pointer;
    animation: slide 1.5s infinite;

    img {
      width: 95px;
      height: 70px;
    }
  }
}

@keyframes slide {
  from {
    top: 90%;
  }

  50% {
    top: 88.5%;
  }

  to {
    top: 90%;
  }
}
</style>
