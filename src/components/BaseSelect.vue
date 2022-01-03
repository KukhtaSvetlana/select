<template>
  <section class="dropdown-wrapper">
    <p @click="isVisible = !isVisible" class="selected-item">
      <span>{{selectedItem}}</span>
      <svg :class="isVisible ? 'dropdown__icon-up' : ''" class="dropdown__icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path fill="none" d="M0 0h24v24H0z"/><path d="M12 10.828l-4.95 4.95-1.414-1.414L12 8l6.364 6.364-1.414 1.414z"/></svg>
    </p>
    <div :class="isVisible ? 'visible' :  'invisible'" class="dropdown-popover">
      <input v-model="searchQuery" type="text" placeholder="Search for User" class="dropdown-popover__search-field">
      <p v-if="filteredUser.length === 0"> No Data Available</p>
      <div class="dropdown-popover__options">
        <ul>
          <li v-for="(user, index) in filteredUser"
              :key="`user-${index}`"
              @click="onSelectItem(user)">{{user.name}}</li>
        </ul>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "BaseSelect",
  data() {
    return {
      searchQuery: '',
      selectedItem: 'Select User',
      isVisible: false,
      users: []
    }
  },
  computed: {
    filteredUser () {
      const query = this.searchQuery.toLowerCase();
      if (this.searchQuery === '') return this.users;
      return this.users.filter((user) => {
        return Object.values(user).some((word) => String(word).toLowerCase().includes(query))
      });
    }
  },
  methods: {
    onSelectItem(user) {
      this.selectedItem = user.name;
      this.isVisible = !this.isVisible;
    },
    receiveUsers () {
      fetch("https://jsonplaceholder.typicode.com/users")
          .then( res => res.json())
          .then( (json) => {
            this.users = json
          })
    }
  },
  mounted() {
   this.receiveUsers();
  }
}
</script>

<style scoped lang="css">
p, ul, li {
  margin: 0;
  padding: 0;
}

li {
  list-style: none;
}

.dropdown-wrapper {
  position: relative;
  max-width: 450px;
  margin: 0 auto;
}

.dropdown-wrapper .selected-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 12px;
  height: 40px;
  font-size: 20px;
  font-weight: 500;
  color: #8a949e;
  border: 2px solid #e8e8e8;
  border-radius: 12px;
}

.dropdown__icon {
  transform: rotate(0deg);
  /*transition: all 0.5s ease;*/
}

.dropdown__icon-up {
  transform: rotate(180deg);
  transition: all 0.5s ease;
}

.dropdown-popover {
  position: absolute;
  top: 45px;
  right: 0;
  left: 0;
  width: 100%;
  border: 2px solid #e8e8e8;
  background-color: #fff;
  border-radius: 0 0 12px 12px;
  box-sizing: border-box;

  visibility: hidden;
  transition: all .5s linear;
  max-height: 0;
  overflow: hidden;
}

.dropdown-popover.visible {
  max-height: 450px;
  visibility: visible;
}

.dropdown-popover .dropdown-popover__search-field {
  padding-left: 5px;
  width: 90%;
  height: 30px;
  border: 2px solid #e8e8e8;
  font-size: 20px;
  font-weight: 500;
}

.dropdown-popover__options ul{
  text-align: left;
  margin: 10px 0;
  padding: 0 20px;
  max-height: 325px;
  overflow-y: auto;
  overflow-x: hidden;
}

.dropdown-popover__options li {
  width: 100%;
  font-size: 18px;
  font-weight: 500;
  border-bottom: 1px solid #e8e8e8;
  padding: 10px;
  box-sizing: border-box;
  cursor: pointer;
}

.dropdown-popover__options li:hover {
  background-color: #d6d6d6;
  color: white;
}

</style>