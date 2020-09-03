<template>
    <div class="min-h-screen bg-grey-darker p-8">
        <div class="max-w-sm mx-auto">
            <hello-world></hello-world>
        </div>
    </div>
</template>

<script>
  import HelloWorld from "@/components/HelloWorld"

  export default {
    components: {
      HelloWorld
    }
  }
</script>

<style src="@/assets/css/app.css"/>