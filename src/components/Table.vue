<template>
    <div>
        <div class="parent_table">
            <div class="searchFields">
                <input @input="filteredTable(table.value)" v-model="search" placeholder="поиск...." type="text">
            </div>
            <div class="div_for_align_boxes">
                <table class="table" border="1">
                    <tr :class="setCustomHeaders()">
                        <td class="static_headers" @click="sortTable(indexCol,header)" v-for="(header,indexCol) in table.rows" :key="indexCol">{{ header }} </td>
                    </tr>
                    <tr :class="[setCustomTdEven(), setCustomTdOdd() ]" v-for="(obj,rowIndex) in collection()" :key="rowIndex">
                        <td class="table_boxes" v-for="(props,colIndex) in  obj" :key="colIndex" v-on:dblclick="editField(rowIndex,colIndex)">
                            <input class="inputEdit" @keyup.esc.prevent="inputEditEsc()" @keyup.enter.prevent="inputEnter(rowIndex,colIndex)" @blur.prevent="wrapperBlur(rowIndex,colIndex)" v-model="inputText" v-if="isEditing(rowIndex,colIndex)">
                            <template v-else> {{ props}}
</template>
                        </td>
                        <td class="no-wrap-section">
                            <div class="addRow-icon" @click.prevent="addRow(tableIndex,rowIndex)">
                                <addRow-icon/>
                            </div>
                        </td>
                    </tr>
                </table>
            </div>
            <div class="table_all_controls">
                <div class="table_btn_controls">
                    <button @click="removeTable(tableIndex)">Удалить таблицу</button>
                    <button @click="cleanTable(tableIndex)">Очистить таблицу</button>
                    <button id="show-modal" @click="saveTab = true">Получить таблицу</button>
                </div>
                <div class="pagination_controls">
                    <button v-for="p in pagination.pages" :class="checkActivePage(p)" :key="p" @click.prevent="setPage(p)">{{ p }}</button>
                </div>
            </div>
        </div>
        <save-buffer v-if="saveTab" :table="this.table.value" :tableIndex="tableIndex" @close="saveTab=false"></save-buffer>
    </div>
</template>

<script src="./Table.js">

</script>

<style lang="scss" scoped>
    $hundred: 100%;
    @mixin flex-param($flex, $x, $y) {
        display: $flex;
        justify-content: $x;
        align-items: $y;
    }

    %tableOptions {
        padding: 10px;
        border: 2px solid gray;
        text-align: center;
    }

    .parent_table {
        display: flex;
        flex-direction: column;
    }

    .btn_search {
        border-radius: 5px;
        box-sizing: border-box;
    }

    .table {
        width: $hundred;
        border-collapse: collapse;
        border: 2px solid gray;
    }

    .div_for_align_boxes {
        @include flex-param(flex, center, center);
        margin-top: 20px;
        width: $hundred;
    }

    .static_headers,
    .table_boxes {
        @extend %tableOptions;
        white-space: pre-wrap;
    }

    .table_all_controls {
        @include flex-param(flex, space-around, flex-end);
        margin-top: 40px;
    }

    .no-wrap-section {
        @extend %tableOptions;
        white-space: nowrap;
    }

    .customize-headers {
        background: #FFFFCC;
    }

    .td-even:nth-child(even) {
        background: #99FF99;
    }

    .td-odd:nth-child(odd) {
        background: #CCFFFF;
    }

    .active {
        background: #FF6633;
    }

    .addRow-icon,
    table:hover {
        cursor: pointer;
    }

    .inputEdit {
        width: 70%;
    }

    .searchFields {
        @include flex-param(flex, space-between, $y: none);
        width: $hundred / 5;
    }
</style>
