<template>
  <section class="bg-gray-100">
    <div class="mx-auto max-w-screen-xl px-4 py-16 sm:px-6 lg:px-8">
      <div class="grid grid-cols-1 gap-x-16 gap-y-8 lg:grid-cols-5">
        <div class="lg:col-span-2 lg:py-12">
          <p class="max-w-xl text-lg">
            Edit menu.
          </p>

          <div class="mt-8">
            <a href="#" class="text-2xl font-bold text-yellow-600"> RasaHaka </a>

            <address class="mt-2 not-italic text-gray-500">Takengon Aceh Indonesia.</address>
          </div>
        </div>

        <div class="rounded-lg bg-white p-8 shadow-lg lg:col-span-3 lg:p-12">
          <form @submit.prevent="submitForm" class="space-y-4">
            <div>
              <label class="sr-only" for="name">Nama menu</label>
              <input
                v-model="form.name"
                class="w-full rounded-lg border-gray-200 p-3 text-sm"
                placeholder="Nama menu"
                type="text"
                name="name"
              />
            </div>

            <div class="grid grid-cols-1 gap-4 sm:grid-cols-2">
              <div>
                <label class="sr-only" for="jenis">Jenis</label>
                <input
                  v-model="form.jenis"
                  class="w-full rounded-lg border-gray-200 p-3 text-sm"
                  placeholder="Jenis"
                  type="text"
                  name="jenis"
                />
              </div>

              <div>
                <label class="sr-only" for="harga">Harga</label>
                <input
                  v-model="form.harga"
                  class="w-full rounded-lg border-gray-200 p-3 text-sm"
                  placeholder="harga"
                  type="number"
                  name="harga"
                />
              </div>
            </div>

            <div>
              <label class="sr-only" for="foto">Foto</label>
              <p class="py-3">Update foto menu <span class="text-gray-400">(biarkan bila tidak ada perubahan).</span></p>
              
              <div>
                <img :src="'/img/products/' + form.foto" alt="" width="200px" style="border-radius: 10px; margin: auto;">  
                <!-- <p style="text-align: center; font-style: italic;" class="text-gray-500">Gambar sebelumnya</p> -->
              </div>

              <input
                ref="file"
                class="w-full rounded-lg border-gray-200 p-3 text-sm"
                placeholder="foto"
                type="file"
                name="foto"
                @change="handleFileChange"
              />
            </div>

            <!-- Hidden Input Field for the product ID -->
            <input type="hidden" v-model="form.id" name="id" />

            <div class="mt-4">
              <button
                type="submit"
                class="inline-block w-full rounded-lg bg-black px-5 py-3 font-medium text-white sm:w-auto"
              >
                Ubah data
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { useForm } from '@inertiajs/vue3';
import Swal from 'sweetalert2'; 

export default {
    props: {
        data: {
            required: true
        }
    },
    setup(props) {
        const form = useForm({
            id: props.data[0].id, // Add the ID to the form data
            name: props.data[0].nama,
            jenis: props.data[0].jenis,
            harga: props.data[0].harga,
            foto: props.data[0].gambar
        });

        // Handle file input change and assign it to form data
        const handleFileChange = (event) => {
            // Directly modify the foto field in the form object
            form.foto = event.target.files[0];
        };

        // Submit form
        const submitForm = () => {
            // Send the form data, including the ID, when posting to the update route
            form.post(route('admin.edit.menu', form.id), {
              onFinish: () => {
                  Swal.fire({
                  icon: 'success',
                  title: 'Diubah !',
                  text: 'Menu berhasil di ubah !.',
                  showConfirmButton: true,
                });
                form.reset(); 
                },
            });
        };

        return {
            form,
            handleFileChange,
            submitForm,
        };
    }
};
</script>
