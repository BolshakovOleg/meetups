<template>
  <div class="filters-panel">
    <div class="filters-panel__col">
      <form-check
        :options="$options.dateFilterOptions"
        v-model="date"
      ></form-check>
    </div>
    <div class="filters-panel__col">
      <div class="form-group form-group_inline">
        <div class="input-group input-group_icon input-group_icon-left">
          <app-icon icon="search" />
          <input
            id="filters-panel__search"
            class="form-control form-control_rounded form-control_sm"
            type="text"
            placeholder="Поиск"
            v-model="search"
          />
        </div>
      </div>
      <div class="form-group form-group_inline">
        <page-tabs :selected.sync="view"></page-tabs>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapMutations } from "vuex";
import { mapFields } from "@/utils";
import AppIcon from "@/components/base/app-icon";
import FormCheck from "@/components/ui/form-check";
import PageTabs from "@/components/ui/page-tabs";

export default {
  name: "meetups-filter",

  dateFilterOptions: [
    { text: "Все", value: "all" },
    { text: "Прошедшие", value: "past" },
    { text: "Ожидаемые", value: "future" }
  ],

  components: {
    AppIcon,
    FormCheck,
    PageTabs
  },

  watch: {
    $route: {
      handler: function() {
        this.setFilterField({
          field: "search",
          value: this.$route.query.search || ""
        });
        this.setFilterField({
          field: "date",
          value: this.$route.query.date || "all"
        });
        this.setView(this.$route.query.view || "list");
      },
      immediate: true
    }
  },

  computed: {
    ...mapState({
      filter: state => state["meetups"].filter
    }),
    ...mapFields(
      ["date", "search"],
      (vm, field) => vm.filter[field],
      (vm, field, value) => {
        vm.setFilterField({ field, value });
        vm.updateRoute();
      }
    ),
    view: {
      get() {
        return this.$store.state["meetups"].view;
      },
      set(value) {
        this.setView(value);
        this.updateRoute();
      }
    }
  },

  methods: {
    ...mapMutations("meetups", {
      setFilterField: "SET_FILTER_FIELD",
      setView: "SET_VIEW"
    }),
    updateRoute() {
      this.$router
        .push({
          path: "/meetups",
          query: this.buildQuery()
        })
        .catch(error => {
          if (error.name !== "NavigationDuplicated") throw error;
        });
    },
    buildQuery() {
      let query = { date: this.date, search: this.search, view: this.view };
      if (this.view === "list") delete query.view;
      if (this.date === "all") delete query.date;
      if (this.search === "") delete query.search;
      return query;
    }
  }
};
</script>

<style scoped></style>
