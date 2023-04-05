<template>
  <div class="min-h-full">
    <div
      class="mx-auto mt-8 grid max-w-3xl grid-cols-1 gap-6 sm:px-6 lg:max-w-7xl"
    >
      <h1 class="font-semibold leading-6 text-gray-900 mb-3 text-2xl">Notes</h1>
      <div class="space-y-6 lg:col-span-2 lg:col-start-1">
        <!-- Description list-->

        <!-- Comments-->
        <section aria-labelledby="notes-title">
          <div class="bg-white shadow sm:overflow-hidden sm:rounded-lg">
            <div class="divide-y divide-gray-200">
              <div class="px-4 py-5 sm:px-6">
                <h2 id="notes-title" class="text-lg font-medium text-gray-900">
                  Notes
                </h2>
              </div>
              <div class="px-4 py-6 sm:px-6">
                <ul role="list" class="space-y-8">
                  <li v-for="comment in note" :key="comment.uid">
          
                    <div class="flex space-x-3" >
                      <div class="flex-shrink-0">
                        <img
                          class="h-10 w-10 rounded-full"
                          :src="`https://images.unsplash.com/photo-1519244703995-f4e0f30006d5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80`"
                          alt=""
                        />
                      </div>
                      <div>
                        <div class="text-sm flex flex-row">
                          <a
                            href="#"
                            class="font-medium text-gray-900"
                            @mouseover="showhover = true"
                            @mouseout="showhover = false"
                            >sankati swetha
                          </a>
                        
                            <PencilSquareIcon
                        
                              class="h-5 w-5 flex-shrink-0 text-gray-400 group-hover:text-gray-500 ml-3 mr-3"
                            @click="openedit(comment.note,comment.uid)"
                            />
                      
                            <TrashIcon
                              class="h-5 w-5 flex-shrink-0 text-gray-400 group-hover:text-gray-500"
                              @click="deleteNotes(comment.uid)"
                            />
                             
                        
                        </div>
                        <div class="mt-1 text-sm text-gray-700">
                          <p>{{ comment.note }}</p>
                        </div>
                        <div class="mt-2 space-x-2 text-sm">
                          <span class="font-medium text-gray-500">{{
                            comment.date
                          }}</span>

                          <button
                            type="button"
                            class="font-medium text-gray-900"
                          >
                            Reply
                          </button>
                        </div>
                      </div>
                    </div>
                  </li>
                </ul>
              </div>
            </div>
            <div class="bg-gray-50 px-4 py-6 sm:px-6">
              <div class="flex space-x-3">
                <div class="flex-shrink-0">
                  <img
                    class="h-10 w-10 rounded-full"
                    :src="user.imageUrl"
                    alt=""
                  />
                </div>
                <div class="min-w-0 flex-1">
                  <!-- <form action="prevent.Default"> -->
                      <form>
                  <div>
                    <label for="comment" class="sr-only">About</label>
                    <textarea
                      id="comment"
                      name="comment"
                      rows="3"
                      class="block w-full rounded-md border-0 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-blue-600 sm:py-1.5 sm:text-sm sm:leading-6"
                      placeholder="Add a note"
                      v-model="notesValue"
                    />
                  </div>
                  <div class="mt-3 flex items-center justify-between">
                    <a
                      href="#"
                      class="group inline-flex items-start space-x-2 text-sm text-gray-500 hover:text-gray-900"
                    >
                      <QuestionMarkCircleIcon
                        class="h-5 w-5 flex-shrink-0 text-gray-400 group-hover:text-gray-500"
                        aria-hidden="true"
                      />
                      <span>Some HTML is okay.</span>
                    </a>
                    <button
                      class="inline-flex items-center justify-center rounded-md bg-blue-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-blue-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blue-600"
                      @click="postData"
                    >
                      Comment
                    </button>
                  </div>
                </form>
                  <!-- </form> -->
                </div>
              </div>
            </div>
          </div>
        </section>
        <div v-if="openEditModal">
        <CollectionEdit :filled_note="prefilledData" :uid="prefilledDataUId" @openModal="openEditModal=false"/>
     
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

import {
  Menu,
  MenuButton,
  MenuItem,
  MenuItems,
  Popover,
  PopoverButton,
  PopoverOverlay,
  PopoverPanel,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";
import {
  ArrowLongLeftIcon,
  CheckIcon,
  HandThumbUpIcon,
  HomeIcon,
  MagnifyingGlassIcon,
  PaperClipIcon,
  QuestionMarkCircleIcon,
  UserIcon,
} from "@heroicons/vue/20/solid";
import {
  Bars3Icon,
  BellIcon,
  XMarkIcon,
  PencilSquareIcon,
  TrashIcon,
} from "@heroicons/vue/24/outline";
const openEditModal=ref(false)
const showhover = ref(false);
const notesValue = ref("");
const comments = ref([]);
const prefilledDataUId=ref("")
const user = {
  name: "Whitney Francis",
  email: "whitney@example.com",
  imageUrl:
    "https://images.unsplash.com/photo-1517365830460-955ce3ccd263?ixlib=rb-=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=8&w=256&h=256&q=80",
};
const prefilledData=ref("")
async function postData() {
  const postoptions = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiN2ZiMzBkNzhmM2NmNGEwZmJiZWNkZjJkOGM2ZjNhMGEiLCJkIjoiMTY4MDA3NyIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNzM0NzR9.QjMEQKeWqKdjLekJkiFGTdhJ3iwilHM5Aa9FEqbWvOI`,
    },
    body: {
      entity_id: "1360",
      project_id: "111",
      note: notesValue,
      entity: "TASKS",

    },
  };
  const data = await useAuthLazyFetchPost(
    "https://v1-orm-lib.mars.hipso.cc/notes/TASKS/1360",
    postoptions
  );

}
 const getData=useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/notes/entity/TASKS/1360?project_id=111&offset=0&limit=100&sort_column=id&sort_direction=desc"
);
const note=ref("")
note.value = getData.data._rawValue;
console.log("note-->",note.value)

function openedit(data:any,id:any){
  console.log("openEditModal.value-->",openEditModal.value)
openEditModal.value=true
prefilledData.value=data
prefilledDataUId.value=id

}
function  deleteNotes(id:any){
    const putOptions = {
    method: "DELETE",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
      Authorization: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiN2ZiMzBkNzhmM2NmNGEwZmJiZWNkZjJkOGM2ZjNhMGEiLCJkIjoiMTY4MDA3NyIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNzM0NzR9.QjMEQKeWqKdjLekJkiFGTdhJ3iwilHM5Aa9FEqbWvOI`,
    },
}
 const addTemplateData = useAuthLazyFetchDelete(
    `https://v1-orm-lib.mars.hipso.cc/notes/${id}`,
  

    putOptions
  );
  note.value.forEach((item,index)=>{
    if(item.uid==id){
      console.log(index)
     note.value.splice(index, 1);
      }})

}
</script>