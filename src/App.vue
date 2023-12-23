<template>
  <div
    class="flex flex-col items-center justify-center min-h-screen"
    v-if="isWindowOk"
  >
    <!-- <img
      src="https://media0.giphy.com/media/mOx00kv61GZgHuCxSj/giphy.gif?cid=6c09b952x883mqem7bsfpc64ovyhzqd3owvp1n6xxxp1ycd6&ep=v1_internal_gif_by_id&rid=giphy.gif&ct=s"
      class="absolute bottom-10 right-[42%] w-auto h-[100px]"
      alt=""
    />
    <img
      src="https://media2.giphy.com/media/sAXNeR5KR0x2rRwY86/giphy.gif?cid=6c09b952s2ez2jy9rbwxcv9wuehk88915q1zza3g7r0g9qu8&ep=v1_stickers_related&rid=giphy.gif&ct=s"
      class="absolute -top-14 left-0 w-auto h-[300px]"
      alt=""
    />
    <img
      src="https://media0.giphy.com/media/nQwvVRQVuPblXBjYVk/giphy.gif?cid=6c09b952nd40rj5684mvybj8r584fy4wr9heiq7erkgkivor&ep=v1_stickers_related&rid=giphy.gif&ct=s"
      alt=""
      class="absolute right-20 top-10 w-auto h-[200px]"
    /> -->
    <div
      @click="downloadImg"
      class="w-[70px] h-[70px] cursor-pointer hover:scale-105 duration-300 rounded-full absolute right-[10%] bg-white flex items-center justify-center shadow-xl"
    >
      <i class="fa-solid fa-download fa-2xl text-pink-400"></i>
    </div>
    <input
      class="hidden"
      type="file"
      id="image"
      accept="image/*"
      v-on:change="loadFile"
    />
    <div
      id="card"
      class="bg-white px-8 py-4 max-sm:w-[80%] sm:h-[300px] sm:w-[500px] rounded-xl shadow-xl border-[0.001px] border-[#00000020] relative"
    >
      <header class="flex justify-between">
        <h1 class="header-card sm:text-5xl text-pink-400 w-fit">NewJeans</h1>
        <div class="bunnies flex gap-2 items-center">
          <h1 class="header sm:text-5xl text-[#1B225B] italic">Bunnies Club</h1>
          <img src="./assets/images/bunny.png" class="w-[50px] h-[50px]" />
        </div>
      </header>

      <img
        @click="triggerInput"
        id="output"
        src="./assets/images/haerin_default.jpg"
        class="heart img-placeholder absolute max-sm:w-[50px] sm:w-[150px] sm:h-[150px] top-20 left-8 rounded-full object-cover"
      />
      <p class="text-[10px] sm:ml-48 py-4 font-semibold max-w-[243px] relative">
        버니 클럽은 친구들과 함께 귀여운 작은 토끼와 놀러 가는 곳입니다. 혼자
        토끼와 놀기도 재미있지만, 친구들과 함께 하는 것이 훨씬 더 즐겁습니다!
        <br />
        <br />
        Bunnies club is a place to go to play with some cute little bunny with
        friends. It's fun playing with bunnies alone, but it's way more fun
        doing it with friends!
        <br />
        <br />
        <br />
        여기 서명함
        <br />
        <span id="editName" @click="triggerText">해린(Haerin) </span>
        <br />
        <span
          ref="elipsisContainer"
          class="dots h-10 inline-block whitespace-nowrap overflow-hidden w-[243px]"
        >
        </span>
        <span class="signature bottom-12 right-10 absolute text-4xl">
          Newjeans
        </span>
      </p>
    </div>
  </div>
  <div v-else class="flex items-center justify-center h-screen gap-4">
    <h1 class="header text-center text-[#1B225B] italic text-7xl">
      Please use desktop
    </h1>
    <img src="./assets/images/bunny.png" class="w-[70px] h-[70px]" />
  </div>
</template>

<script>
import domtoimage from "dom-to-image";
import { saveAs } from "file-saver";

export default {
  name: "App",
  data() {
    return {
      name: null,
      isWindowOk: window.innerWidth > 768,
    };
  },
  mounted() {
    window.addEventListener("resize", () => {
      this.isWindowOk = window.innerWidth > 768;
    });
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.handleResize);
  },
  methods: {
    handleResize() {
      this.isScreenSizeBelow768 = window.innerWidth <= 768;
    },
    loadFile(event) {
      let image = document.getElementById("output");
      console.log(event);
      image.src = URL.createObjectURL(event.target.files[0]);
    },
    triggerInput() {
      document.getElementById("image").click();
    },
    triggerText() {
      let el = document.getElementById("editName");

      el.classList.add("active");
      el.contentEditable = true;

      el.addEventListener("keypress", (event) => {
        if (event.key === "Enter") {
          event.preventDefault();
        }
      });

      el.addEventListener("blur", () => {
        el.classList.remove("active");
        this.name = el.innerText;
        el.contentEditable = false;
      });

      el.focus();
    },
    downloadImg() {
      let node = document.getElementById("card");
      let name = this.name;
      console.log(name);
      domtoimage
        .toBlob(node, {
          height: node.offsetHeight * 2,
          width: node.offsetWidth * 2,
          style: {
            transform: "scale(2)",
            transformOrigin: "top left",
            width: node.offsetWidth + "px",
            height: node.offsetHeight + "px",
          },
        })
        .then((blob) => {
          window.saveAs(blob, name + "'s Card.png");
        })
        .catch((error) => {
          console.error("Oops, something went wrong!", error);
        });
    },
  },
};
</script>
