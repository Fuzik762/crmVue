<template lang='pug'>
form.form-create-task(@submit.prevent='submitCreateTask')
  h4 Новая задача
  .input-field
    input#title.validate(v-model="title" type='text' required)
    label(for='title') Название задачи   
  .input-field
    textarea#textarea2.materialize-textarea(v-model="description" data-length='2048')
    label(for='textarea2') Описание задачи
    span.character-counter(style='float: right; font-size: 12px;') {{+description.length}}/2048
  input.datepicker(type='text' ref="datepicker")
  button.btn.light-blue.darken-4(type='submit') Создать задачу


</template>

<script>
export default {
  data() {
    return {
      title: '',
      description: '',
      date: null,
      redirectTab: 'Список',
    }
  },
  mounted() {
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: 'dd.mm.yyyy',
      defaultDate: new Date(),
      setDefaultDate: true
    });
  },
  methods: {
    async submitCreateTask() {
      const task = {
        title: this.title,
        description: this.description,
        date: Date.parse(this.date.date),
        status: 'Выполняется',
        statusColor: 'cyan',
      }
      try {
        await this.$store.dispatch('createTask', task)
        this.$emit('showList', this.redirectTab)
      } catch (error) {}
      
    }
  },
  destroyed() {
    if(this.date && this.date.destroy) {
      this.date.destroy()
    }
  },
}
</script>

<style lang="scss" scoped>
  .form-create-task {
    max-width: 50%;
    margin-top: 50px;
  }
  .btn { 
    margin-top: 20px;
  }
</style>