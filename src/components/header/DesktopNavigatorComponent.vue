<template>
  <div id="wrapper_nav" v-if="!isMobile" class="d_flex_row j_content_center">
    <ul
      class="none_decor_ul d_flex_row flex_wrap j_content_space_around f_family_sans w_100"
      id="non_drop"
    >
      <li
        v-for="nav in naviPanel($store.state.currentLanguage)"
        :key="nav.value"
        :id="nav.idEl"
        class=""
      >
        <div v-if="nav.value != 5 && nav.value != 4">
          <router-link
            :to="nav.linkTo"
            @click="deleteDataFromFilter(nav.linkTo)"
            @mouseover="drawHorizontalLine(nav.value, '.navigation_sites')"
            @mouseout="clearHorizontalLine(nav.value, '.navigation_sites')"
            class="upper_case none_text_decor nav_link_color navigation_sites"
            style="display: block"
            :class="{
              'f_weight_bold_700 color_black ': whatTitleIsit(nav.txt),
            }"
          >
            {{ nav.txt }}
            <div class="horizontal_line_hover"></div>
          </router-link>
        </div>
        <div
          v-else
          class="d_flex_column upper_case nav_link_color"
          style="display: block"
          :class="{
            'f_weight_bold_700 color_black ': whatTitleIsit(nav.txt),
          }"
        >
          <div
            class="pad_b1em c_pointer navigation_sites"
            @mouseover="drawHorizontalLine(nav.value, '.navigation_sites')"
            @mouseout="clearHorizontalLine(nav.value, '.navigation_sites')"
            @click="getNeedNavigate(nav.value)"
          >
            {{ nav.txt }}
            <div class="horizontal_line_hover"></div>
          </div>

          <div
            v-if="isTeam && nav.value == 4"
            class="p_absolute z_20 font_1 teamss"
          >
            <ul class="pad_0 none_decor_ul_no_pad">
              <li class="t_left" v-for="team in nav.linkTo" :key="team.value">
                <a
                  class="upper_case none_text_decor nav_link_color navigation_sites"
                  :href="'/team' + team.linkTo"
                  >{{ team.txt }}
                </a>
                <hr class="mar_top_bot" />
              </li>
            </ul>
          </div>
          <div
            v-if="isEvent && nav.value == 5"
            class="p_absolute z_20 font_1 teamss"
          >
            <ul class="pad_0 none_decor_ul_no_pad">
              <li class="t_left" v-for="team in nav.linkTo" :key="team.value">
                <a
                  class="upper_case none_text_decor nav_link_color navigation_sites"
                  :href="'/archive' + team.linkTo"
                  >{{ team.txt }}</a
                >

                <hr class="mar_top_bot" />
              </li>
            </ul>
          </div>
        </div>
      </li>
    </ul>
    <div class="dropdown">
      <div class="dropbtn nav_link_color d_flex_row p_abs">
        <div class="open_sans f_weight_bold upper_case" @click="setDarkLight()">
          <svg
            v-if="!dropMenu"
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="currentColor"
            class="bi bi-list"
            viewBox="0 0 16 16"
          >
            <path
              fill-rule="evenodd"
              d="M2.5 12a.5.5 0 0 1 .5-.5h10a.5.5 0 0 1 0 1H3a.5.5 0 0 1-.5-.5zm0-4a.5.5 0 0 1 .5-.5h10a.5.5 0 0 1 0 1H3a.5.5 0 0 1-.5-.5zm0-4a.5.5 0 0 1 .5-.5h10a.5.5 0 0 1 0 1H3a.5.5 0 0 1-.5-.5z"
            />
          </svg>
          <svg
            v-else
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="currentColor"
            class="bi bi-x-lg"
            viewBox="0 0 16 16"
          >
            <path
              d="M2.146 2.854a.5.5 0 1 1 .708-.708L8 7.293l5.146-5.147a.5.5 0 0 1 .708.708L8.707 8l5.147 5.146a.5.5 0 0 1-.708.708L8 8.707l-5.146 5.147a.5.5 0 0 1-.708-.708L7.293 8 2.146 2.854Z"
            />
          </svg>
        </div>
      </div>
      <ul
        v-if="dropMenu"
        class="none_decor_ul f_family_sans d_flex_column flex_wrap j_content_center dropdown-content"
        id="drop"
      >
        <li
          v-for="nav in naviPanel($store.state.currentLanguage)"
          :key="nav.value"
          :id="nav.idEl"
          class="d_flex_row_reverse j_content_space_around w_75 dropdown_li"
        >
          <a
            v-if="nav.value != 5 && nav.value != 4"
            :href="nav.linkTo"
            @click="deleteDataFromFilter(nav.linkTo)"
            class="upper_case none_text_decor nav_link_color navigation_sites w_50 t_left"
            style="display: block"
            :class="{
              'f_weight_bold_700 color_black ': whatTitleIsit(nav.txt),
            }"
          >
            {{ nav.txt }}
            <div class="horizontal_line_hover"></div>
          </a>
          <div
            v-else
            class="d_flex_column upper_case none_text_decor w_50"
            style="display: block"
            :class="{
              'f_weight_bold_700 color_black ': whatTitleIsit(nav.txt),
            }"
          >
            <div
              class="c_pointer navigation_sites d_flex_row j_content_start"
              @mouseover="drawHorizontalLine(nav.value, '.navigation_sites')"
              @mouseout="clearHorizontalLine(nav.value, '.navigation_sites')"
              @click="getNeedNavigate(nav.value)"
            >
              <div class="d_inline">
                <span>
                  {{ nav.txt }}
                </span>
                <span class="v_align_super">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="12"
                    height="12"
                    fill="currentColor"
                    class="bi bi-caret-down-fill"
                    viewBox="0 0 16 16"
                  >
                    <path
                      d="M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z"
                    />
                  </svg>
                </span>
              </div>
              <div class="horizontal_line_hover"></div>
            </div>

            <div v-if="isTeam && nav.value == 4" class="font_1 teamss">
              <ul class="pad_0 none_decor_ul_no_pad">
                <li
                  class="t_left f_weight_bold_700 small_font_07"
                  v-for="team in nav.linkTo"
                  :key="team.value"
                >
                  <a
                    class="upper_case none_text_decor nav_link_color navigation_sites"
                    :href="'/team' + team.linkTo"
                    >{{ team.txt }}
                  </a>
                  <hr class="mar_top_bot" />
                </li>
              </ul>
            </div>
            <div v-if="isEvent && nav.value == 5" class="font_1 teamss">
              <ul class="pad_0 none_decor_ul_no_pad">
                <li
                  class="t_left f_weight_bold_700 small_font_07"
                  v-for="team in nav.linkTo"
                  :key="team.value"
                >
                  <a
                    class="upper_case none_text_decor nav_link_color navigation_sites"
                    :href="'/archive' + team.linkTo"
                    >{{ team.txt }}</a
                  >

                  <hr class="mar_top_bot" />
                </li>
              </ul>
            </div>
          </div>
        </li>
      </ul>
      <div class="spinner" v-if="spiner">
        <SpinerComponent />
      </div>
    </div>
  </div>
</template>
      <script>
import SpinerComponent from "@/components/helpers/SpinerComponent.vue";
export default {
  name: "DesktopNavigatorComponent",
  components: {
    SpinerComponent,
  },
  data() {
    return {
      isMobile: false,
      navigationData: null,
      dropMenu: false,
      spiner: false,
      intrval: null,
      isTeam: false,
      isEvent: false,
      isBoth: {},
      offert: [],
      listNavi: [
        { value: "головна", inner: [] },
        { value: "афіша", inner: [] },
        { value: "новини", inner: [] },
        { value: "про театр", inner: [] },
        {
          value: "люди",
          inner: ["автори", "режисери", "актори", "команда"],
        },
        { value: "архів", inner: ["всі події", "новини", "вистави"] },
        { value: "партнери", inner: [] },
        { value: "профіль", inner: [] },
        { value: "контакти", inner: [] },
        { value: "проект 'трикутник'", inner: [] },
      ],
      listNaviTeam: ["автори", "режисери", "актори", "команда"],
      listNaviArch: ["всі події", "новини", "вистави"],
      listNaviEn: [
        { value: "main", inner: [] },
        { value: "poster", inner: [] },
        { value: "news", inner: [] },
        { value: "about", inner: [] },
        {
          value: "people",
          inner: ["authors", "directors", "actors", "team"],
        },
        { value: "archive", inner: ["all events", "news", "plays"] },
        { value: "partners", inner: [] },
        { value: "profile", inner: [] },
        { value: "contacts", inner: [] },
        { value: "project 'triangle'", inner: [] },
      ],
      watchData: 0,
    };
  },

  created() {
    this.naviPanel(this.$store.state.currentLanguage);
    this.eventScrollClick();
  },
  methods: {
    drawHorizontalLine(index, classEl) {
      // Підкреслення по наведенню на елемент
      let navEl = document.querySelectorAll(classEl);

      let widthElem = navEl[index].offsetWidth - 2;
      let cnt = 1;
      this.intrval = setInterval(() => {
        if (navEl[index].firstElementChild) {
          navEl[index].firstElementChild.style.width = String(cnt) + "px";
        }

        if (cnt >= Number(widthElem)) {
          clearInterval(this.intrval);
          return;
        }
        cnt += 3;
      }, 5);
    },
    clearHorizontalLine(index, classEl) {
      // Скасування по прибиранню курсора миші на елемент
      let navEl = document.querySelectorAll(classEl);

      clearInterval(this.intrval);
      if (navEl[index].firstElementChild) {
        navEl[index].firstElementChild.style.width = 0;
      }
    },

    deleteDataFromFilter(lnk = "", newWindow = false) {
      // Видаляє зі сховища дату для фільтрів
      // Скидує фільтри по місяцям
      if (!newWindow) {
        localStorage.removeItem("dataYM");
        this.dropMenu = !this.dropMenu;
        this.spiner = true;
      } else {
        localStorage.removeItem("dataYM");
        this.openLinkNewWindow(lnk);
      }
    },
    getNeedNavigate(val) {
      if (val == 5) {
        this.isEvent = !this.isEvent;
      } else if (val == 4) {
        this.isTeam = !this.isTeam;
      }
    },
    whatTitleIsit(nameNav) {
      // Перевіряряє назву сторінки і поле навігації
      let namePage = document.querySelector("title").innerHTML;
      return String(nameNav).toLocaleLowerCase() ===
        String(namePage).toLocaleLowerCase()
        ? true
        : false;
    },

    naviPanel(language) {
      let tempListNavigate;
      tempListNavigate = this.listNaviEn;
      if (language == 0) {
        tempListNavigate = this.listNavi;
      }

      let listNaviLinks = [
        ["/"],
        ["/plays"],
        ["/news"],
        ["/about"],
        ["%2Fauthors", "%2Fdirectors", "%2Factors", "%2Fmain-team"],
        // ["/archive-all"],
        ["%2Farchive-all", "%2Farchive-news", "%2Farchive-plays"],
        ["/our_partners"],
        ["/my_profile"],
        ["/contacts"],
        ["/triangle-for-ukrainian-artists"],
      ];

      let dataListNavi = [];
      for (let x = 0; x < tempListNavigate.length; x++) {
        if (listNaviLinks[x].length == 1) {
          dataListNavi.push({
            value: x,
            txt: tempListNavigate[x].value,
            idEl: "naviLink" + String(x),
            linkTo: listNaviLinks[x][0],
          });
        } else {
          dataListNavi.push({
            value: x,
            txt: tempListNavigate[x].value,
            idEl: "naviLink" + String(x),
            linkTo: this.gnerateObjectLinks(listNaviLinks[x], x, language),
          });
        }
      }
      return dataListNavi;
    },

    gnerateObjectLinks(list, counter, language) {
      //

      let tempListNavigate;
      tempListNavigate = this.listNaviEn;
      if (language == 0) {
        tempListNavigate = this.listNavi;
      }
      let tempList = [];
      for (let x = 0; x < list.length; x++) {
        tempList.push({
          value: x,
          txt: tempListNavigate[counter].inner[x],
          // idEl: "naviLinkInner" + String(x) + list[x].replace("/", ""),
          linkTo: list[x],
        });
      }

      return tempList;
    },

    eventScrollClick() {
      // Прибирає drop menu при прокрутці
      document.addEventListener("scroll", () => {
        if (this.dropMenu) {
          this.dropMenu = false;
          document.querySelector(".dropdown").style.height = "0";
        }
      });
    },
    setDarkLight() {
      // Затемняє фон після випадання меню
      this.dropMenu = !this.dropMenu;
      if (this.dropMenu) {
        document.querySelector(".dropdown").style.height = "100vh";
        document.querySelector(".dropdown").style.width = "100%";
      } else {
        document.querySelector(".dropdown").style.height = "0";
      }
    },
    openLinkNewWindow(lnk) {
      window.open(lnk, "_blank").focus();
    },
    goToTeamList(lnk, pathTo) {
      this.$router.push({
        name: pathTo,
        params: {
          slug: lnk,
        },
      });
      // .then(() => {
      //   location.reload();
      // });
    },
  },
};
</script>
<style scoped>
@media screen and (max-width: 1700px) {
  #non_drop {
    padding: 0 12em;
  }
}
@media screen and (max-width: 1500px) {
  #non_drop {
    padding: 0 12em;
  }
}

@media screen and (max-width: 1350px) {
  #non_drop {
    padding: 0 9em;
  }
}
@media screen and (max-width: 1300px) {
  #non_drop {
    padding: 0 6em;
  }
}
@media screen and (max-width: 1100px) {
  #non_drop {
    padding: 0 3em;
  }
}
@media screen and (max-width: 1000px) {
  #non_drop {
    display: none;
  }
  #wrapper_nav {
    flex-direction: column;
  }
  #drop {
    padding: 85px 0;
    width: 90vw;
    height: max-content;
  }
  .dropdown {
    display: inline-block !important;
    position: absolute !important;
    top: 7.5%;
    left: 0%;
    background-color: #fffffff6;
    z-index: 12;
  }
}

.none_decor_ul {
  list-style: none;
  padding: 0 16.5em 0 16.5em;
}

.dropbtn {
  color: rgb(43, 43, 43);
  padding: 16px;
  font-size: 16px;
  cursor: pointer;
  width: max-content;
}

.dropdown {
  position: relative;
  display: none;
}

.dropdown-content {
  position: sticky;
  background: linear-gradient(#ffffffe9, #ffffffdb);
  min-width: 100%;
  height: max-content;
  z-index: 11;
  left: 0;
}

.dropdown-content li {
  color: black;
  padding: 7px 16px;
  text-decoration: none;
  /* display: block; */
  /* width: 100%; */
}

.dropdown-content a:hover {
  background-color: #ffffff;
}

.animate_drop {
  transition: padding-left 0.5s ease-out;
}

.animate_drop:hover {
  padding-left: 10px;
}
.teamss {
  background-color: rgb(254, 254, 254);
  border-radius: 7px;
  padding: 7px;
}
.spinner {
  margin-top: 45%;
}
</style>