<script setup>
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { useForm } from '@inertiajs/inertia-vue3';
import { ref } from 'vue';

const nameInput = ref(null);
const descriptionInput = ref(null);

const form = useForm({
    name: '',
    description: '',
});

const createProduct = () => {
    form.post(route('products.store'), {
        preserveScroll: true,
        onSuccess: () => form.reset(),
        onError: () => {
            if (form.errors.name) {
                form.reset('name', 'description');
                nameInput.value.focus();
            }
            if (form.errors.description) {
                form.reset('description');
                descriptionInput.value.focus();
            }
        },
    })
}

</script>

<template>
    <section>
        <header>
            <h2 class="text-lg font-medium text-gray-900">Create Product</h2>

            <p class="mt-1 text-sm text-gray-600">
                Enter product name
            </p>
        </header>

        <form @submit.prevent="createProduct" class="mt-6 space-y-6">
            <div>
                <InputLabel for="name" value="Name" />

                <TextInput
                    id="name"
                    ref="nameInput"
                    v-model="form.name"
                    type="text"
                    class="mt-1 block w-full"
                />

                <InputError :message="form.errors.name" class="mt-2" />
            </div>


            <div>
                <InputLabel for="description" value="Description" />

                <TextInput
                    id="description"
                    ref="descriptionInput"
                    v-model="form.description"
                    type="text"
                    class="mt-1 block w-full"
                />

                <InputError :message="form.errors.description" class="mt-2" />
            </div>

            <div class="flex items-center gap-4">
                <PrimaryButton :disabled="form.processing">Save</PrimaryButton>

                <Transition enter-from-class="opacity-0" leave-to-class="opacity-0" class="transition ease-in-out">
                    <p v-if="form.recentlySuccessful" class="text-sm text-gray-600">Saved.</p>
                </Transition>
            </div>

        </form>
    </section>
</template>



