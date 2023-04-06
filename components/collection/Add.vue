<template>
      <div class="space-y-6 lg:col-span-2 lg:col-start-1">
        <section aria-labelledby="notes-title">
          <div class="bg-white shadow sm:overflow-hidden sm:rounded-lg">
            <div class="bg-gray-50 px-4 py-6 sm:px-6">
              <div class="flex space-x-3">
                <div class="flex-shrink-0">
                  <img
                    class="h-10 w-10 rounded-full"
                    src="https://images.unsplash.com/photo-1517365830460-955ce3ccd263?ixlib=rb-=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=8&w=256&h=256&q=80"
                    alt=""
                  />
                </div>
                <div class="min-w-0 flex-1">
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
               
                </div>
              </div>
            </div>
          </div>
        </section>
      </div>
</template>

<script setup lang="ts">
import { ref,defineEmits } from "vue";
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
const notesValue = ref("");
const emit = defineEmits(["postData"]);
function postData(){
   emit("postData",notesValue)
}
</script>