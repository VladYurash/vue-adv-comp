<template>
    <div class="min-h-screen bg-grey-darker p-8">
        <div class="max-w-sm mx-auto">
            <contact-list :pseudo-slot="({ contact }) => contact.name.first">
                <template v-slot="{ contact }">
                    <a :href="`/contacts/${contact.id}`">
                        {{ contact.name.first }}
                    </a>
                </template>
            </contact-list>
        </div>
    </div>
</template>

<script>
  import ContactList from '@/components/ContactList'

  export default {
    components: {
      ContactList,
    },
    data() {
      return {}
    }
  }
</script>

<style src="@/assets/css/app.css"/>
