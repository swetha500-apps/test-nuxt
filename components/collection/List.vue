<template>
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
            </template>

<script setup lang="ts">
import { ref, defineProps, defineEmits } from "vue";
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
const prefilledDataUId=ref("")
const prefilledData=ref("")
 const getData=useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/notes/entity/TASKS/1360?project_id=111&offset=0&limit=100&sort_column=id&sort_direction=desc",""
);
const note=ref("")
note.value = getData.data._rawValue;
const emit = defineEmits(["openModal","deleteNotes"]);
function openedit(data:any,id:any){
openEditModal.value=true
prefilledData.value=data
prefilledDataUId.value=id
emit("openModal",{prefilledData,prefilledDataUId})
}
function  deleteNotes(id:any){
 emit("deleteNotes",id)

}
</script>