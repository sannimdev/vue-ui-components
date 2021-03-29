<template>
    <div>
        <b-dropdown :text="dropdownLabel" ref="dropdown" class="dropdown m-2">
            <b-dropdown-form @submit.stop.prevent class="form-dropdown">
                <b-form-input
                    v-model="keyword"
                    placeholder="Search for your keyword"
                    class="input-full-width"
                    autocomplete="off"
                    @keypress.enter="clearKeyword"
                />
            </b-dropdown-form>
            <ul class="dropdown-items">
                <li
                    v-for="item in liveSearch"
                    :key="item.id"
                    @click="toggleItem(item.id)"
                    :class="item.disabled && 'disabled'"
                >
                    <label>
                        <b-checkbox
                            class="checkbox"
                            :class="isMulti ? 'multi' : 'single'"
                            :checked="item.checked"
                            :disabled="item.disabled"
                            @change="toggleItem(item.id)"
                        />
                        <span>{{ ellipsis(item.label) }}</span>
                    </label>
                </li>
            </ul>
            <div>
                <p class="search-none" v-show="liveSearch.length === 0">
                    There is no any item.
                </p>
                <p
                    v-show="isMulti && isAllSelectable"
                    class="multi-controller"
                    @click="selectMulticheckableAll"
                >
                    <label>
                        <b-checkbox
                            class="checkbox"
                            @change="selectMulticheckableAll"
                            :checked="isSelectAll"
                        />
                        <span>전체 선택</span>
                    </label>
                </p>
            </div>
        </b-dropdown>
    </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { DropdownItem } from '@/types';

export default Vue.extend({
    name: 'Dropdown',
    props: {
        /*
            **타입스크립트에서 객체 형태로 컴포넌트를 정의할 때 props 의 타입으로 타입스크립트의 타입을 사용할 수 없다는 이 치명적인 단점은 현재 해답을 찾지 못했다.** 
            진행중인 프로젝트에서는 말단 베이스 컴포넌트에 해당하는 컴포넌트들만 객체 형태로 컴포넌트를 정의하고 있는데 이 문제로 타입을 사용하지 않고 자바스크립트 기본 타입으로 풀어서 받는다.
            https://ui.toast.com/weekly-pick/ko_20190327
        */
        isMulti: { type: Boolean, default: true },
        items: { type: Array, required: true },
    },
    data() {
        return {
            keyword: '',
            parsedItems: [] as DropdownItem[],
        };
    },
    computed: {
        liveSearch(): DropdownItem[] {
            const items = this.items as DropdownItem[];
            return items.filter(
                (item) =>
                    item.label.toLowerCase().indexOf(this.keyword.trim()) > -1
            );
        },
        dropdownLabel(): string {
            const items = this.items as DropdownItem[];
            const checkedItems = items.filter(({ checked }) => checked);
            return checkedItems.length > 0
                ? this.ellipsis(
                      checkedItems.map((item) => item.label).join(',')
                  )
                : this.isMulti
                ? 'Select items'
                : 'Select an item';
        },
        isAllSelectable(): boolean {
            return (
                this.liveSearch.filter(({ disabled }) => disabled).length !==
                this.liveSearch.length
            );
        },
        isSelectAll(): boolean {
            return (
                this.liveSearch.filter((item) => !item.disabled).length ===
                this.liveSearch.filter((item) => item.checked).length
            );
        },
    },
    created() {
        this.parsedItems = this.items.concat() as DropdownItem[];
    },
    methods: {
        toggleItem(id: number): void {
            const item = this.parsedItems.find((item) => item.id === id);
            if (item?.disabled) return;

            if (this.isMulti) {
                this.parsedItems = this.parsedItems.map((item) =>
                    item.id === id ? { ...item, checked: !item.checked } : item
                );
            } else {
                // 기존 아이템이 체크된 상태인지 확인하기
                const checked = this.parsedItems.find((item) => item.id === id)
                    ?.checked;
                if (checked) {
                    // 기존 아이템이 체크되어 있으면 체크를 모두 해제하고
                    this.parsedItems = this.parsedItems.map((item) => ({
                        ...item,
                        checked: false,
                    }));
                } else {
                    // 기존 아이템이 체크되어 있지 않으면 나머지 아이템의 체크를 취소하고 해당 아이템만 체크한다.
                    this.parsedItems = this.parsedItems.map((item) => ({
                        ...item,
                        checked: item.id === id,
                    }));
                }
            }
            this.doUpdate(this.parsedItems);
        },
        ellipsis(label: string): string {
            const maxLength = 30;
            if (label.length > maxLength) {
                return label.substring(0, maxLength) + '...';
            }
            return label;
        },
        selectMulticheckableAll(): void {
            // 전체 선택하기
            this.parsedItems = this.parsedItems.map((item) =>
                item.disabled ? item : { ...item, checked: !this.isSelectAll }
            );
            this.doUpdate(this.parsedItems);
        },
        doUpdate(items: DropdownItem[]): void {
            this.$emit('update', items);
        },
        clearKeyword() {
            this.keyword = '';
        },
    },
});
</script>

<style lang="scss" scoped>
$cell_hover: #f1f3f5;
$cell_disabled: #e9ecef;

.dropdown {
    /* width: 500px; */
    &,
    .dropdown-items {
        width: 300px;
    }

    .form-dropdown form {
        outline: none;
        margin: 0;
        padding: 0;
        .input-full-width {
            width: 100%;
            border-top: none;
            border-left: none;
            border-right: none;
            border-radius: 0;
            transition: border linear 0.4s;
            &:focus {
                border-bottom: 1px solid #4dabf7;
                box-shadow: none !important;
                outline: none !important;
            }
        }
    }
    .dropdown-items {
        margin: 0 !important;
        padding: 0 !important;
        max-height: 250px;
        overflow-y: auto;
        margin: 0;
        padding: 1rem;
        li {
            list-style-type: none;
            display: flex;
            align-items: center;
            flex-direction: row;
            user-select: none;
            padding: 0.5rem;
            line-height: 1.5rem;
            transition: background linear 0.2s;

            &:hover {
                background-color: $cell_hover;
            }

            &.disabled {
                background-color: $cell_disabled;
                &,
                & > * {
                    cursor: not-allowed;
                }
            }
        }
    }
    p.search-none {
        text-align: center;
        margin: 0.5rem 0;
    }

    p.multi-controller {
        padding: 0.5rem 0.5rem;
        margin: 0;

        &:hover {
            background-color: $cell_hover;
        }
    }

    label {
        margin: 0;
        .checkbox {
            display: inline-block;
        }

        span {
            flex: 1;
            text-overflow: ellipsis;
            word-break: break-all;
        }
    }
}
</style>

