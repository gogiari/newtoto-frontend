<template>
    <div>
        <div class="headerview">
            <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="50" height="50" />
            <div style="display: flex;">
                <span style="margin-right: 30px;" class="headermenu">Green Sports</span>
                <nav>
                    <RouterLink to="/">Home</RouterLink> |
                    <div class="dropdown-container" v-if="showDropdown">
                        <div class="dropdown-wrapper">
                            <ul class="dropdown-list">
                                <li v-for="menu in menuList" :key="menu.menu_id">{{ menu.menu_name }}</li>
                            </ul>
                        </div>
                    </div>
                    <a class="menus" @click="toggleDropdown">Community</a>
                </nav>
                <div class="theme-sel" @click="toggleTheme">
                    <font-awesome-icon v-if="isSunIconVisible" :icon="['fas', 'sun']" size="2xl" style="color: #ffffff;" />
                    <font-awesome-icon v-else :icon="['fas', 'moon']" size="2xl" style="color: #ffffff;" />
                </div>
            </div>
        </div>
    </div>
</template>
<script lang="ts">
import axios from 'axios';

interface MenuItem {
  menu_id: number;
  menu_name: string;
  menu_seq: number;
  // 다른 속성들도 필요하다면 여기에 추가
}

export default {
    data() {
        return {
            isSunIconVisible: false,
            isBlueBackground: false,
            showDropdown: false,
            menuList: [] as MenuItem[],
        };
    },
    methods: {
        toggleTheme() {
            this.isSunIconVisible = !this.isSunIconVisible;
        },
        toggleDropdown() {
            this.showDropdown = !this.showDropdown;
        },
        async fetchMenuList() {
            try {
                const respone = await axios.get('http://localhost:8090/menu/list');
                this.menuList = respone.data;
                this.showDropdown = true;
                console.log( respone.data)
            } catch (error) {
                console.error('메뉴 리스트 불러오기 에러', error);
            }
        }
    },
    mounted() {
        this.fetchMenuList();
    }
};
</script>
<style>
.headerview {
    display: grid;
    grid-template-columns: 50px auto;
    /* background-color: #3CB371; */
    border-bottom: 3px solid #3CB371;
}

.headermenu {
    color: #3CB371;
    font-size: 30px;
    font-weight: bold;
}

.theme-sel {
    float: right;
    width: 30px;
    border-radius: 50%;
}

.theme-sel:hover {
    background-color: antiquewhite;
    transition: 0.5s;
}

.dropdown-container {
    position: relative;
    display: inline-block;
}

.dropdown-wrapper {
    /* 텍스트가 한 줄로 유지되도록 설정 */
    white-space: nowrap;
}

.dropdown-list {
    /* 가로 방향 배치
    display: flex;
    flex-direction: row; */

    position: absolute;
    top: 100%;
    left: 0;
    background-color: white;
    list-style: none;
    padding: 5px;
    border: 1px solid #3CB371;
    border-radius: 10px;
    transition: max-height 3s ease-in-out;
}
.menus:hover {
    cursor: pointer;
}
</style>
