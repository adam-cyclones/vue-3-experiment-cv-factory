
<script setup>
  import { ref, useTemplateRef } from 'vue';

  const tagsInput = useTemplateRef();
  const tagsUsed = ref([]);
  const TAGS_INPUT_NAME = 'tags';
  const CV_FILE_NAME = "cv_file";

  /**
   * Cannot use v-model because I need to do something more advanced with the form.
  */
  const handleSubmit = (e) => {
    const elForm = e.target;
    const form = new FormData(elForm);

    const newValue = form.get(TAGS_INPUT_NAME);
    tagsUsed.value.push(newValue);

    elForm.reset();
  }
</script>

<template>
  <div class="hello">
    <form @submit.prevent="handleSubmit">
      <div>
        <label class="block" :for="CV_FILE_NAME">Upload a CV</label>
        <input :id="CV_FILE_NAME" type="file" accept=".md"/>
      </div>
      <div class="pt-2">
        <label class="block" :for="TAGS_INPUT_NAME">Tags</label>
        <input :id="TAGS_INPUT_NAME" :name="TAGS_INPUT_NAME" :ref="tagsInput" required class="form-control" type="text" placeholder="skills..."/>
        <button type="addTag">Write</button>
      </div>
    </form>

  </div>
  <div class="tag-wrapper">
    <div v-if="tagsUsed.length"></div>
    <span class="tag" v-for="tag in tagsUsed" :key="tag">
      {{tag}}
    </span>
  </div>
</template>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.tag-wrapper {
  display: flex;
  flex-wrap: wrap;
  gap: .5rem;
  margin: 2rem auto;
  max-width: 31.25rem;
  width: 100%;
}

.tag {
  background-color: beige;
  border-radius: .25rem;
  border: 1px solid whitesmoke;
  padding: .5rem 1rem;
}

.block {
  display: block;
}

.pt-2 {
  padding-top: 2rem;
}
</style>
