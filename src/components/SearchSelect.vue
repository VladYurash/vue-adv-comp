<template>
    <onclick-outside :action="close">
        <div class="search-select" :class="{ 'is-active': isOpen }">
            <button @click="open" type="button" class="search-select-input" ref="btn">
                <span v-if="value !== null">{{ value }}</span>
                <span v-else class="search-select-placeholder">Select a band...</span>
            </button>
            <div v-show="isOpen" class="search-select-dropdown" ref="dropdown">
                <input
                    class="search-select-search"
                    v-model="search"
                    ref="search"
                    @keydown.esc="close"
                    @keydown.up="highlightPrev"
                    @keydown.down="highlightNext"
                    @keydown.enter.prevent="selectHighlighted"
                    @keydown.tab.prevent
                >
                <ul v-show="filteredOpts.length > 0" class="search-select-options" ref="list">
                    <li class="search-select-option" :class="{ 'is-active': index === highlighted }"
                        v-for="(option, index) in filteredOpts"
                        :key="option"
                        @click="select(option)"
                    >{{ option }}</li>
                </ul>
                <div v-show="filteredOpts.length === 0" class="search-select-empty">No results found for <b>{{ search }}</b></div>
            </div>
        </div>
    </onclick-outside>
</template>

<script>
  import OnclickOutside from '@/components/OnclickOutside'

  export default {
    name: 'SearchSelect',
    components: {
      OnclickOutside
    },
    props: ['value', 'options', 'filterFunction'],
    data() {
      return {
        isOpen: false,
        search: '',
        highlighted: 0
      }
    },
    computed: {
      filteredOpts() {
        return this.filterFunction(this.search, this.options)
      }
    },
    methods: {
      open() {
        if (this.isOpen) return
        this.isOpen = true
        this.$nextTick(() => {
          this.$refs.search.focus()
          this.scrollToHighlighted()
        })
      },
      close() {
        if (!this.isOpen) return
        this.isOpen = false
        this.$refs.btn.focus()
      },
      select(option) {
        this.$emit('input', option)
        this.search = ''
        this.highlighted = 0
        this.close()
      },
      scrollToHighlighted() {
        this.$refs.list.children[this.highlighted].scrollIntoView({block: 'nearest'})
      },
      selectHighlighted() {
        this.select(this.filteredOpts[this.highlighted])
      },
      highlight(index) {
        this.highlighted = index
        if (this.highlighted < 0) this.highlighted = this.filteredOpts.length - 1
        if (this.highlighted > this.filteredOpts.length - 1) this.highlighted = 0
        this.scrollToHighlighted()
      },
      highlightPrev() {
        this.highlight(this.highlighted -= 1)
      },
      highlightNext() {
        this.highlight(this.highlighted += 1)
      },
    }
  }
</script>

<style>
    .search-select-options::-webkit-scrollbar {
        width: 5px;
        background-color: #3d4852;

    }
    .search-select-options::-webkit-scrollbar-thumb {
        background-color: #606f7b;
        border-radius: 5px;
    }
</style>
