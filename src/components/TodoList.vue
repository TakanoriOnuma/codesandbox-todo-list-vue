<template lang="pug">
.todo-list
  h1.title todolist
  transition-group(name="filp-list", tag="ul")
    template(v-for="todo in todoList")
      li.todo(
        :key="todo.id",
        :class="{ 'todo--done': todo.isDone }"
      )
        .todo__line
          button.todo__status(
            type="button",
            @click="$emit('statusChange', todo.id)"
          )
            | {{ todo.isDone ? 'DONE' : 'DOING' }}
          .todo__deadline 締切：{{ todo.deadline | formatDate }}
          .todo__delete(
            @click="$emit('delete', todo.id)"
          )
        .todo__text {{ todo.text }}
</template>

<script>
export default {
  props: {
    todoList: {
      type: Array,
      default: function() {
        return [];
      }
    }
  },
  filters: {
    formatDate(date) {
      const year = date.getFullYear();
      const month = ('00' + (date.getMonth() + 1)).slice(-2);
      const day = ('00' + date.getDate()).slice(-2);
      return `${year}-${month}-${day}`;
    }
  }
};
</script>

<style lang="scss" scoped>
.todo-list {
  margin-top: 15px;
  padding: 10px;
  border-radius: 5px;
  border: solid 1px #ccc;
}
.title {
  text-align: center;
}

// 本当はこれでflipアニメーションができるのだが上手くいかない
.flip-list-move {
  transition: transform 0.5s;
}
.flip-list-leave-active {
  position: absolute;
}

.todo {
  $root: &;
  border: solid 1px #ccc;
  padding: 10px;
  background-color: #fff;
  transition: transform 0.5s;

  & + & {
    margin-top: 10px;
  }

  &__line {
    display: flex;
    justify-content: space-between;
  }

  &__status {
    width: 60px;
    padding: 5px;
    line-height: 1;
    border-radius: 5px;
    background-color: #ccf;
    border: solid 1px #aaf;
    flex: 0 0 auto;
    transition: background-color 0.5s;

    #{$root}--done & {
      background-color: #eee;
      border-color: #ccc;
    }
  }

  &__deadline {
    flex: 1 1 auto;
    padding-right: 10px;
    text-align: right;
  }

  &__delete {
    position: relative;
    width: 30px;
    flex: 0 0 auto;
    &::before, &::after {
      position: absolute;
      top: 50%;
      left: 50%;
      width: 70%;
      height: 2px;
      background-color: #ccc;
      content: ''
    }
    &::before {
      transform: translate3d(-50%, -50%, 0) rotate(45deg);
    }
    &::after {
      transform: translate3d(-50%, -50%, 0) rotate(135deg); 
    }
  }

  &__text {
    margin-top: 5px;
  }
}
</style>

