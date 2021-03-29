<template>
    <div id="app">
        <div class="columns">
            <div class="column-left">
                <h3>Select 🧚‍♂️artists you like</h3>
                <dropdown :items="users" @update="onUserUpdate" />
                <b-list-group style="max-width: 300px; margin: 0 auto">
                    <b-list-group-item
                        :key="user.id"
                        v-for="user in selectedUsers"
                        class="d-flex align-items-center"
                    >
                        <b-avatar class="mr-3"></b-avatar>
                        <span class="mr-auto">{{ user.label }}</span>
                    </b-list-group-item>
                </b-list-group>
            </div>
            <div class="column-center">
                <h3>You cannot 🍟choose everything.</h3>
                <dropdown :items="foods" @update="onUserUpdate" />
            </div>
            <div class="column-right">
                <h3>Select an 🎧album you favorite</h3>
                <dropdown
                    :isMulti="false"
                    :items="albums"
                    @update="onAlbumUpdate"
                />
                <b-card
                    v-if="selectedAlbum"
                    title="Your favorite album is..."
                    img-src="https://picsum.photos/600/300/?image=25"
                    img-alt="Your favorite album"
                    img-top
                    tag="article"
                    style="max-width: 20rem; margin: 0 auto"
                    class="mb-2"
                >
                    <b-card-text>
                        {{ selectedAlbum.label }}
                    </b-card-text>

                    <b-button href="#" variant="warning" @click="deselectAlbum"
                        >Cancel</b-button
                    >
                </b-card>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue';
import Dropdown from '@/components/Dropdown.vue';
import { DropdownItem } from './types';

export default Vue.extend({
    name: 'App',
    components: {
        Dropdown,
    },
    data() {
        return {
            users: [
                {
                    id: 1,
                    label: 'Bret',
                    value: 'Bret',
                    checked: false,
                },
                {
                    id: 2,
                    label: 'Antonette',
                    value: 'Antonette',
                    checked: false,
                },
                {
                    id: 3,
                    label: 'Samantha',
                    value: 'Samantha',
                    checked: false,
                },
                {
                    id: 4,
                    label: 'Karianne',
                    value: 'Karianne',
                    checked: false,
                    disabled: true,
                },
                {
                    id: 5,
                    label: 'Kamren',
                    value: 'Kamren',
                    checked: false,
                    disabled: true,
                },
                {
                    id: 6,
                    label: 'Leopoldo_Corkery',
                    value: 'Leopoldo_Corkery',
                    checked: false,
                },
                {
                    id: 7,
                    label: 'Elwyn.Skiles',
                    value: 'Elwyn.Skiles',
                    checked: false,
                },
                {
                    id: 8,
                    label: 'Maxime_Nienow',
                    value: 'Maxime_Nienow',
                    checked: false,
                },
                {
                    id: 9,
                    label: 'Delphine',
                    value: 'Delphine',
                    checked: false,
                },
                {
                    id: 10,
                    label: 'Moriah.Stanton',
                    value: 'Moriah.Stanton',
                    checked: false,
                },
            ] as DropdownItem[],
            foods: [
                {
                    id: 1,
                    label: 'Spagetti',
                    disabled: true,
                },
                {
                    id: 2,
                    label: 'Pizza',
                    disabled: true,
                },
                {
                    id: 3,
                    label: 'Chicken',
                    disabled: true,
                },
                {
                    id: 4,
                    label: 'Salad',
                    disabled: true,
                },
                {
                    id: 5,
                    label: 'Whopper',
                    disabled: true,
                },
            ] as DropdownItem[],
            albums: [
                {
                    id: 1,
                    label: 'quidem molestiae enim',
                    value: 'quidem molestiae enim',
                },
                {
                    id: 2,
                    label: 'sunt qui excepturi placeat culpa',
                    value: 'sunt qui excepturi placeat culpa',
                },
                {
                    id: 3,
                    label: 'omnis laborum odio',
                    value: 'omnis laborum odio',
                    disabled: true,
                },
                {
                    id: 4,
                    label: 'non esse culpa molestiae omnis sed optio',
                    value: 'non esse culpa molestiae omnis sed optio',
                },
                {
                    id: 5,
                    label: 'eaque aut omnis a',
                    value: 'eaque aut omnis a',
                },
                {
                    id: 6,
                    label: 'natus impedit quibusdam illo est',
                    value: 'natus impedit quibusdam illo est',
                    disabled: true,
                },
                {
                    id: 7,
                    label: 'quibusdam autem aliquid et et quia',
                    value: 'quibusdam autem aliquid et et quia',
                },
                {
                    id: 8,
                    label: 'qui fuga est a eum',
                    value: 'qui fuga est a eum',
                },
                {
                    id: 9,
                    label: 'saepe unde necessitatibus rem',
                    value: 'saepe unde necessitatibus rem',
                },
                {
                    id: 10,
                    label: 'distinctio laborum qui',
                    value: 'distinctio laborum qui',
                },
                {
                    id: 11,
                    label: 'quam nostrum impedit mollitia quod et dolor',
                    value: 'quam nostrum impedit mollitia quod et dolor',
                },
                {
                    id: 12,
                    label: 'consequatur autem doloribus natus consectetur',
                    value: 'consequatur autem doloribus natus consectetur',
                },
                {
                    id: 13,
                    label: 'ab rerum non rerum consequatur ut ea unde',
                    value: 'ab rerum non rerum consequatur ut ea unde',
                },
                {
                    id: 14,
                    label: 'ducimus molestias eos animi atque nihil',
                    value: 'ducimus molestias eos animi atque nihil',
                },
                {
                    id: 15,
                    label:
                        'ut pariatur rerum ipsum natus repellendus praesentium',
                    value:
                        'ut pariatur rerum ipsum natus repellendus praesentium',
                },
                {
                    id: 16,
                    label:
                        'voluptatem aut maxime inventore autem magnam atque repellat',
                    value:
                        'voluptatem aut maxime inventore autem magnam atque repellat',
                },
                {
                    id: 17,
                    label: 'aut minima voluptatem ut velit',
                    value: 'aut minima voluptatem ut velit',
                },
                {
                    id: 18,
                    label: 'nesciunt quia et doloremque',
                    value: 'nesciunt quia et doloremque',
                },
                {
                    id: 19,
                    label: 'velit pariatur quaerat similique libero omnis quia',
                    value: 'velit pariatur quaerat similique libero omnis quia',
                },
            ] as DropdownItem[],
        };
    },
    computed: {
        selectedUsers(): DropdownItem[] {
            console.log(this.users.filter(({ checked }) => checked));
            return this.users.filter(({ checked }) => checked);
        },
        selectedAlbum(): DropdownItem {
            return this.albums.find(({ checked }) => checked) as DropdownItem;
        },
    },
    methods: {
        onUserUpdate(users: DropdownItem[]): void {
            this.users = users;
        },
        onAlbumUpdate(album: DropdownItem[]): void {
            this.albums = album;
        },
        deselectAlbum() {
            this.albums = this.albums.map((album) => ({
                ...album,
                checked: false,
            }));
        },
    },
});
</script>

<style lang="scss">
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    font-size: 18px;

    .columns {
        display: flex;
        flex-direction: row;

        .column-left,
        .column-center,
        .column-right {
            flex: 1;
        }
    }
}

/* 부트스트랩 체크박스 => 동그랗게 */
.dropdown-menu.show {
    padding-bottom: 0;
    .custom-control.checkbox.single {
        & > .custom-control-label {
            &::before {
                border-radius: 50% !important;
            }
        }
    }
}
</style>
