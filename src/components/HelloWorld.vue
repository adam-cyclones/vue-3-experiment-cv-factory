
<script setup>
  import { ref, useTemplateRef, defineExpose } from 'vue';

  const elTagsInput = useTemplateRef('elTagsInput');
  const elJDTextarea = useTemplateRef('elJDTextarea')
  const tagsUsed = ref([]);
  const formError = ref(null);

  const JOB_DESCRIPTION_NAME = 'jd';
  const TAGS_INPUT_NAME = 'tags';
  const CV_FILE_NAME = "cv_file";
  
  const strings = {
    startOverText: 'Start over',
    clipboardButtonText: 'Load from clipboard',
    uploadFileInstruction: 'Upload a CV',
    fileHelpText: 'Only markdown is supported.',
    labelJD: 'Job Description',
    labelTags: 'Tags',
    commonAdd: 'Add',
    skillsHelpText: 'Add your skills to see them apear here..'
  }
  /**
   * Cannot use v-model because I need to do something more advanced with the form.
  */
  const handleSubmit = (e) => {
    const elInput = elTagsInput.value;
    const elForm = e.target;

    if (!elInput.checkValidity()) {
      formError.value = 'Error, tags must not be empty'.
      return;
    }

    // should always have a value
    const newValue = elInput.value;

    tagsUsed.value.push(newValue);
    elForm.reset();
  }

  const handlePasteJD = async () => {
    const clipText = await navigator.clipboard.readText()
    elJDTextarea.value.innerText = clipText
  }

  defineExpose({ strings });
</script>

<template>
  <section>
    <button type="button">{{strings.startOverText}}</button>
  </section>
  <div>
    {{formError}}
  </div>
  <div class="main-wrapper">
    <form novalidate @submit.prevent="handleSubmit">
      <div>
        <label class="block" :for="JOB_DESCRIPTION_NAME">{{strings.labelJD}}</label>
        <textarea ref="elJDTextarea" rows="10" class="block w-100" :name="JOB_DESCRIPTION_NAME"></textarea>
        
        <button type="button" @click="handlePasteJD">{{strings.clipboardButtonText}}</button>
      </div>
      <div class="mt-2">
        <label class="block" :for="CV_FILE_NAME">{{strings.uploadFileInstruction}}</label>
        <input :id="CV_FILE_NAME" type="file" accept=".md"/>
        <p>{{strings.fileHelpText}}</p>
      </div>
      <div class="mt-2">
        <label class="block" :for="TAGS_INPUT_NAME">{{strings.labelTags}}</label>
        <input :id="TAGS_INPUT_NAME" :name="TAGS_INPUT_NAME" ref="elTagsInput" required="required" class="form-control" type="text" placeholder="skills..."/>
        <button type="submit">{{ strings.commonAdd }}</button>
      </div>
    </form>

  </div>
  <div class="tag-wrapper">
    <p class="w-100 text-center" v-if="!tagsUsed.length">{{strings.skillsHelpText}}</p>
    <button type="button" class="tag" v-for="tag in tagsUsed" :key="tag">
      {{tag}}
    </button>
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
  cursor: pointer;
}

.block {
  display: block;
}

.text-center {
  text-align: center;
  display: block;
}

.w-100 {
  width: 100%;
}

.pt-2 {
  padding-top: 2rem;
}

.mt-2 {
  margin-top: 2rem;
}

.main-wrapper {
  max-width: 50rem;
  margin: 0 auto;
}
</style>
