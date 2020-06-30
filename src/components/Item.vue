<template>
  <div class="timeline-item">
    <div class="timeline-img"></div>

    <div class="timeline-content" @click="openCard(id, sizeWidth)">
      <div class="content-date">{{ date }}</div>
      <img class="timeline-logo" :src="logo" :alt="alt" />
      <h2 class="text-hidden">{{ name }}</h2>
      <p>{{ description }}</p>
      <button class="btn-plus" @click="revealCard($event)">
        Plus de détails
      </button>
    </div>

    <transition name="modal">
      <div id="card" class="item-card" v-if="visibleCard === true">
        <h2>{{ name }}</h2>
        <div class="content-date card-date">{{ date }}</div>
        <p>{{ description }}</p>
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          @click="closeCard()"
        >
          <path
            d="M23.954 21.03l-9.184-9.095 9.092-9.174-2.832-2.807-9.09 9.179-9.176-9.088-2.81 2.81 9.186 9.105-9.095 9.184 2.81 2.81 9.112-9.192 9.18 9.1z"
          />
        </svg>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "Item",
  props: ["name", "date", "logo", "description", "alt", "visibleCard", "id"],
  data() {
    return {
      sizeWidth: 0
    };
  },
  methods: {
    revealCard(event) {
      let pathParent = event.path[1];
      let content = document.querySelectorAll(".timeline-content");

      if (pathParent.className !== "timeline-content active") {
        content.forEach(element => {
          if (element.className === "timeline-content active") {
            element.childNodes[3].style.maxHeight = "20px";
            element.classList.remove("active");
            element.childNodes[4].textContent = "Plus de détails";
          }
        });

        pathParent.childNodes[3].style.maxHeight = "600px";
        pathParent.classList.add("active");
        pathParent.childNodes[4].textContent = "Moins de détails";
      } else {
        pathParent.childNodes[3].style.maxHeight = "20px";
        pathParent.classList.remove("active");
        pathParent.childNodes[4].textContent = "Plus de détails";
      }
    },
    size() {
      this.sizeWidth =
        window.innerWidth ||
        document.documentElement.clientWidth ||
        document.body.clientWidth;
    },
    onResize() {
      let content = document.querySelectorAll(".timeline-content");

      this.size();

      content.forEach(element => {
        if (this.sizeWidth < 1024) {
          if (element.className === "timeline-content") {
            element.childNodes[3].style.maxHeight = "14px";
          }

          if (element.className === "timeline-content active") {
            element.childNodes[3].style.maxHeight = "14px";
            element.classList.remove("active");
          }
        } else {
          if (element.className === "timeline-content") {
            element.childNodes[3].style.maxHeight = "20px";
          }

          if (element.className === "timeline-content active") {
            element.childNodes[3].style.maxHeight = "20px";
            element.classList.remove("active");
          }
        }
      });
    },
    openCard(id) {
      const html = document.getElementsByTagName("html")[0];
      const overlay = document.getElementById("overlay");

      let visible = this.visibleCard;

      if (this.sizeWidth < 1024 && id === id) {
        overlay.style.display = "block";
        visible = true;
        this.$emit("update:visibleCard", visible);
        html.setAttribute("style", "overflow-y: hidden !important");
      }
    },
    closeCard() {
      const html = document.getElementsByTagName("html")[0];
      const overlay = document.getElementById("overlay");

      let visible = this.visibleCard;

      if (this.visibleCard) {
        visible = false;
        this.$emit("update:visibleCard", visible);
        html.style.overflowY = "";
        overlay.style.display = "none";
      }
    }
  },

  mounted() {
    // Register an event listener when the Vue component is ready
    this.size();
    window.addEventListener("resize", this.onResize);
  },

  beforeDestroy() {
    // Unregister the event listener before destroying this Vue instance
    window.removeEventListener("resize", this.onResize);
  }
};
</script>

<style lang="scss">
@import "../scss/variables";

.timeline-item {
  width: 100%;
  margin-bottom: 20px;

  &:nth-child(even) {
    .timeline-content {
      float: none;
    }
  }

  &::after {
    content: "";
    display: block;
    clear: both;
  }
}

.timeline-content {
  position: relative;
  width: auto;
  max-width: 100%;
  margin-left: 40px;
  margin-right: 20px;
  padding: 5px 10px;
  border-radius: 4px;
  background: $secondary-text;
  box-shadow: 0 20px 25px -15px rgba(0, 0, 0, 0.3);
  transition: height 0.2s ease;

  .content-date {
    color: $primary-text;
    font-size: 12px;
  }

  .timeline-logo {
    position: absolute;
    top: 25px;
    left: 7px;
    width: 35px;
    height: 35px;
  }

  h2 {
    margin-left: 40px;
    margin-top: 5px;
    font-size: 22px;
  }

  p {
    font-size: 14px;
    color: $light-text;
    margin-left: 40px;
    margin-top: 5px;
    overflow: hidden;
    max-height: 14px;
    transition: max-height 0.8s ease;
  }

  .btn-plus {
    display: none;
  }
}

.timeline-img {
  width: 10px;
  height: 10px;
  background: $primary;
  border-radius: 50%;
  position: absolute;
  left: 50px;
  margin-top: 30px;
  margin-left: -35px;
}

.item-card {
  color: #eee;
  background-color: $dark-primary;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
  z-index: 100;
  width: 90%;
  padding: 10px;

  h2 {
    color: $primary;
    font-size: 28px;
    font-weight: bold;
    width: 80%;
  }

  .card-date {
    margin-top: 10px;
    font-size: 15px;
  }

  p {
    text-indent: 15px;
    margin-top: 25px;
    line-height: 20px;
    font-size: 18px;
    word-spacing: 3px;
  }

  svg {
    position: absolute;
    top: 10px;
    left: calc(100% - (25px + 15px));
    cursor: pointer;
    fill: $primary;
    width: $svg-size;
    height: $svg-size;
  }
}

/* Transition Vue JS */
.modal-enter,
.modal-leave-to {
  opacity: 0;
  transform: translateX(-50%) translateY(-50%) scale(0.1);
}

.modal-enter-to,
.modal-leave {
  opacity: 1;
  transform: translateX(-50%) translateY(-50%) scale(1);
}

.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.25s, transform 0.25s;
}
</style>
