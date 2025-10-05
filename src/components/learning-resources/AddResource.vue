<template>
    <base-dialog v-if="shouldShowErrorDialog" title="Invalid input" @close="closeDialog">
        <template v-slot:default>
            <p>Unfortunately, at least one input value is invalid</p>
            <p>Please check all inputs and make sure you enter at least a few characters into each input field</p>
        </template>
        <template #actions>
            <base-button @click="closeDialog">Ok</base-button>
        </template>
    </base-dialog>
    <base-card>
        <form @submit.prevent="submitData">
            <div class="form-control">
                <label for="title">Title</label>
                <input id="title" name="title" type="text" ref="title">
            </div>
            <div class="form-control">
                <label for="description">Description</label>
                <textarea id="description" name="description" rows="3" ref="description"></textarea>
            </div>
            <div class="form-control">
                <label for="link">Link</label>
                <input id="link" name="link" type="url" ref="link">
            </div>
            <div>
                <base-button type="submit">Add Resource</base-button>
            </div>
        </form>
    </base-card>
</template>

<script>

export default {
    data() {
        return {
            shouldShowErrorDialog: false,
        };
    },
    methods: {
        submitData() {
            const title = this.$refs.title.value;
            const description = this.$refs.description.value;
            const link = this.$refs.link.value;

            if (this.isEmpty(title) || this.isEmpty(description) || this.isEmpty(link)) {
                this.shouldShowErrorDialog = true;
                return;
            }

            this.shouldShowErrorDialog = false;
            this.addResource(title, description, link);
        },
        isEmpty(string) {
            return string.trim().length === 0;
        },
        closeDialog() {
            this.shouldShowErrorDialog = false
        }
    },
    inject: ['addResource']
}
</script>

<style scoped>
label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
}

input,
textarea {
    display: block;
    width: 100%;
    font: inherit;
    padding: 0.15rem;
    border: 1px solid #ccc;
}

input:focus,
textarea:focus {
    outline: none;
    border-color: #3a0061;
    background-color: #f7ebff;
}

.form-control {
    margin: 1rem 0;
}
</style>