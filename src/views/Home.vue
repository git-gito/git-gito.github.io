<template>
    <div class="max-w-2xl mx-auto py-8 px-4">
        <input v-model="search" type="text" placeholder="Search templates..."
            class="mb-6 w-full px-3 py-2 border rounded" />
        <div v-for="template in filteredTemplates" :key="template.name">
            <Template :template="template" :selectedTemplate="selectedTemplate" @select="selectTemplate" />
        </div>
    </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import Template from './../components/Template.vue'

const templates = ref([])

onMounted(async () => {
    try {
        const requestOptions = {
            method: "GET",
            redirect: "follow"
        };

        fetch("https://raw.githubusercontent.com/git-gito/registry-gito/main/templates.json", requestOptions)
            .then((response) => response.text())
            .then((result) => {
                const res = JSON.parse(result);
                // Map each template to have { name, title, description }
                templates.value = res.templates.map(tmpl => ({
                    name: tmpl.tmpl,
                    title: tmpl.tmpl,
                    owner: tmpl.owner,
                    description: tmpl.desc
                }));
            })
            .catch((error) => console.error(error));
    } catch (err) {
        console.error('Failed to fetch templates:', err)
    }
})

const selectedTemplate = ref(null)
const search = ref('')

function selectTemplate(template) {
    selectedTemplate.value = template
}

const filteredTemplates = computed(() =>
    templates.value.filter(t =>
        t.title.toLowerCase().includes(search.value.toLowerCase()) ||
        t.description.toLowerCase().includes(search.value.toLowerCase()) ||
        t.name.toLowerCase().includes(search.value.toLowerCase())
    )
)
</script>
