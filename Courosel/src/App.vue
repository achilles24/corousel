<script>
export default {
  data() {
    return {
      cards: ["1", "2", "3", "4", "5"],
    };
  },
  methods: {
    toggleSlider: function (index) {
      console.log(index);
      $(".carousel").addClass("show");
      $(".carousel").attr("aria-hidden", "false");
      var $num = $(".card-carousel .my-card").length;
      var $even = $num / 2;
      var $odd = ($num + 1) / 2;

      $(".card-carousel .my-card:nth-child(" + index + ")").addClass("active");
      $(".card-carousel .my-card:nth-child(" + index + ")")
        .prev()
        .addClass("prev");
      $(".card-carousel .my-card:nth-child(" + index + ")")
        .next()
        .addClass("next");

      // if ($num % 2 == 0) {
      //   $(".card-carousel .my-card:nth-child(" + $even + ")").addClass(
      //     "active"
      //   );
      //   $(".card-carousel .my-card:nth-child(" + $even + ")").attr(
      //     "tabindex",
      //     "0"
      //   );
      //   $(".card-carousel .my-card:nth-child(" + $even + ")").focus();
      //   $(".card-carousel .my-card:nth-child(" + $even + ")")
      //     .prev()
      //     .addClass("prev");
      //   $(".card-carousel .my-card:nth-child(" + $even + ")")
      //     .next()
      //     .addClass("next");
      // } else {
      //   $(".card-carousel .my-card:nth-child(" + $odd + ")").addClass("active");
      //   $(".card-carousel .my-card:nth-child(" + $odd + ")").attr(
      //     "tabindex",
      //     "0"
      //   );
      //   // let ele = $(".card-carousel .my-card.active");
      //   // ele[0].inert = false;
      //   $(".card-carousel .my-card:nth-child(" + $odd + ")").focus();
      //   $(".card-carousel .my-card:nth-child(" + $odd + ")")
      //     .prev()
      //     .addClass("prev");
      //   $(".card-carousel .my-card:nth-child(" + $odd + ")")
      //     .next()
      //     .addClass("next");
      // }
    },
    closeCard: function () {
      $(".card-carousel").animate({ left: "0px" });
      $(".card-carousel .my-card").siblings().removeClass("prev active next");
      $(".carousel").removeClass("show");
      $(".card-carousel .my-card").attr("tabindex", "-1");
      $(".carousel").attr("aria-hidden", "false");
    },
    setNextTabindex: function () {
      $(".card-carousel .my-card.next").attr("tabindex", "0");
      // let tabEle = $(".card-carousel .my-card.next");
      // tabEle[0].inert = false;
    },
  },
  mounted() {
    $(".card-carousel .my-card").on("click", function () {
      if ($(".card-carousel").is(":animated")) {
        return;
      }

      var $slide = $(".card-carousel .active").width();

      if ($(this).hasClass("next")) {
        $(".card-carousel").animate({ left: "-=" + $slide });
      } else if ($(this).hasClass("prev")) {
        $(".card-carousel").animate({ left: "+=" + $slide });
      }

      $(this).removeClass("prev next");
      $(this).siblings().removeClass("prev active next");

      $(this).addClass("active");
      $(this).prev().addClass("prev");
      $(this).next().addClass("next");
    });

    $(".card-carousel .my-card").keydown(function (e) {
      if (e.keyCode == 9) {
        if ($(".card-carousel").is(":animated")) {
          return;
        }

        var $slide = $(".card-carousel .active").width();

        if ($(this).hasClass("next")) {
          $(".card-carousel").animate({ left: "-=" + $slide });
        } else if ($(this).hasClass("prev")) {
          $(".card-carousel").animate({ left: "+=" + $slide });
        }

        $(this).removeClass("prev next");
        $(this).siblings().removeClass("prev active next");

        $(this).addClass("active");
        $(this).prev().addClass("prev");
        $(this).next().addClass("next");
      }
    });

    // click to close button event
    $(".card-carousel .close-btn").on("click", function (event) {
      event.preventDefault();
      event.stopPropagation();
      $(".card-carousel").animate({ left: "0px" });
      $(".card-carousel .my-card").siblings().removeClass("prev active next");
      $(".carousel").removeClass("show");
      $(".card-carousel .my-card").attr("tabindex", "-1");
    });

    // Enter to close button event
    // $(".card-carousel.my-card.active.close-btn").on(
    //   "mouseenter",
    //   function (event) {
    //     event.preventDefault();
    //     event.stopPropagation();
    //     console.log("Enter pressed");
    //     $(".card-carousel").animate({ left: "0px" });
    //     $(".card-carousel .my-card").siblings().removeClass("prev active next");
    //     $(".carousel").removeClass("show");
    //   }
    // );

    // Keyboard nav
    $("html body").keydown(function (e) {
      if (e.keyCode == 37) {
        // left
        $(".card-carousel .active").prev().trigger("click");
      } else if (e.keyCode == 39) {
        // right
        $(".card-carousel .active").next().trigger("click");
      }
    });
  },
};
</script>

<template>
  <main>
    <div>
      <button @click="toggleSlider(1)">Show Corousel</button>
      <button @click="toggleSlider(2)">Show Corousel</button>
      <button @click="toggleSlider(3)">Show Corousel</button>
      <button @click="toggleSlider(4)">Show Corousel</button>
    </div>
    <section class="carousel" aria-hidden="false">
      <div class="card-carousel">
        <div v-for="card in cards" v-bind:key="card" class="my-card">
          {{ card }}
          <span
            v-on:keyup.enter="closeCard"
            class="close-btn"
            role="button"
            aria-label="Close Button"
            tabindex="0"
            >x</span
          >
          <button class="add-btn" @focus="setNextTabindex">Add</button>
        </div>
      </div>
    </section>
  </main>
</template>

<style>
@import "./assets/base.css";

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding-top: 50px;
  font-weight: normal;
}

main {
  position: relative;
  height: 100%;
  width: 100%;
  padding-top: 50px;
}

.carousel {
  width: 100%;
  display: none;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  font-family: "Quicksand", sans-serif;
  overflow-x: hidden;
  position: relative;
  /* top: -2.5rem; */
}

.carousel.show {
  display: flex;
}

.carousel .card-carousel {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}
.carousel .card-carousel .my-card {
  height: 20rem;
  width: 170px;
  position: relative;
  z-index: 1;
  -webkit-transform: scale(0.6) translateY(-2rem);
  transform: scale(0.6) translateY(-2rem);
  opacity: 0;
  cursor: pointer;
  pointer-events: none;
  background: #2e5266;
  background: linear-gradient(to top, #2e5266, #6e8898);
  transition: 1s;
}

.carousel .card-carousel .my-card .close-btn {
  position: absolute;
  display: none;
  right: 12px;
  background-color: white;
  border-radius: 50%;
  width: 24px;
  padding-left: 8px;
}

.carousel .card-carousel .my-card.active .close-btn {
  display: inline;
}
.carousel .card-carousel .my-card.active {
  z-index: 3;
  -webkit-transform: scale(1) translateY(0) translateX(0);
  transform: scale(1) translateY(0) translateX(0);
  opacity: 1;
  pointer-events: auto;
  transition: 1s;
}
.carousel .card-carousel .my-card.prev,
.carousel .card-carousel .my-card.next {
  z-index: 2;
  opacity: 0.6;
  pointer-events: auto;
  transition: 1s;
}

.carousel .card-carousel .my-card.next {
  -webkit-transform: scale(0.8) translateY(-1rem) translateX(0rem);
  transform: scale(0.8) translateY(-1rem) translateX(0rem);
}

.carousel .card-carousel .my-card.prev {
  -webkit-transform: scale(0.8) translateY(-1rem) translateX(0rem);
  transform: scale(0.8) translateY(-1rem) translateX(0rem);
}

.add-btn {
  position: relative;
  top: 80%;
  left: 50%;
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }
}

[inert] {
  pointer-events: none;
  cursor: default;
}

[inert],
[inert] * {
  user-select: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
}
</style>
