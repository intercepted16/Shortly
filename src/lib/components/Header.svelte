<script lang="ts">
  import { Button } from '$ui/button';
  import { Sun, Moon } from 'lucide-svelte';
  import {
    Cloud,
    CreditCard,
    Github,
    Keyboard,
    LifeBuoy,
    LogOut,
    Mail,
    MessageSquare,
    Plus,
    PlusCircle,
    Settings,
    User,
    UserPlus,
    Users
  } from 'lucide-svelte';
  import { setMode, resetMode } from 'mode-watcher';
  import { type SupabaseClient, type Session } from '@supabase/supabase-js';
  import '$lib/stringExtensions';
  import * as DropdownMenu from '$lib/components/ui/dropdown-menu';
  import * as AlertDialog from '$ui/alert-dialog';
  export let data: {
    supabase: SupabaseClient<any, 'public', any>;
    session: Session | null;
  };
  let { supabase, session } = data;
  $: ({ supabase, session } = data);
  const name = data.session?.user.email.slice(0, 2) ?? null;
  let hover: boolean = false;
  let open: boolean = false;
</script>

<header
  class="sticky top-0 z-50 w-full border-b border-border/40 bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60">
  <div class="container flex items-center h-14 max-w-screen-2xl">
    <div class="hidden mr-4 md:flex">
      <a href="/" class="flex items-center mr-6 space-x-2">
        <img src="/img/favicon.webp" alt="" class="w-8 h-8" />
        <span class="hidden font-bold sm:inline-block">shortly</span></a>
      <nav class="flex items-center gap-6 text-sm">
        <a href="/contact" class="transition-colors hover:text-foreground/80 text-foreground/60"
          >Contact</a>
        <a
          href="/docs/components"
          class="transition-colors hover:text-foreground/80 text-foreground/60">Components</a>
        <a href="/themes" class="transition-colors hover:text-foreground/80 text-foreground/60"
          >Themes</a>
        <a href="/examples" class="transition-colors hover:text-foreground/80 text-foreground/60"
          >Examples</a>
        <a
          href="https://github.com/huntabyte/shadcn-svelte"
          target="_blank"
          rel="noopener noreferrer"
          class="hidden transition-colors text-foreground/60 hover:text-foreground/80 lg:block"
          data-svelte-h="svelte-tibn2o">GitHub</a>
      </nav>
    </div>
    <button
      type="button"
      class="inline-flex items-center justify-center px-0 py-2 mr-2 text-base font-medium transition-colors rounded-md whitespace-nowrap focus-visible:outline-none focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50 hover:text-accent-foreground h-9 hover:bg-transparent focus-visible:bg-transparent focus-visible:ring-0 focus-visible:ring-offset-0 md:hidden"
      ><svg
        stroke-width="1.5"
        viewBox="0 0 24 24"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        class="w-5 h-5"
        ><path
          d="M3 5H11"
          stroke="currentColor"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"></path
        ><path
          d="M3 12H16"
          stroke="currentColor"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"></path
        ><path
          d="M3 19H21"
          stroke="currentColor"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"></path
        ></svg>
      <span class="sr-only" data-svelte-h="svelte-1t6m3jq">Toggle Menu</span></button>
    <div class="flex items-center justify-between flex-1 space-x-2 md:justify-end">
      {#if !session}
        <div class="mx-4 space-x-2">
          <Button href="/authorize">Signup</Button>
          <Button href="/authorize">Login</Button>
        </div>
      {:else}
        <AlertDialog.Root bind:open>
          <AlertDialog.Content>
            <AlertDialog.Header>
              <AlertDialog.Title>Are you sure?</AlertDialog.Title>
              <AlertDialog.Description>
                If you don't remember your credentials, you wont be able to login again.
              </AlertDialog.Description>
            </AlertDialog.Header>
            <AlertDialog.Footer>
              <AlertDialog.Cancel>No, cancel</AlertDialog.Cancel>
              <AlertDialog.Action on:click={async () => await supabase.auth.signOut()}
                >Yes, I'm sure</AlertDialog.Action>
            </AlertDialog.Footer>
          </AlertDialog.Content>
        </AlertDialog.Root>
        <DropdownMenu.Root>
          <DropdownMenu.Trigger asChild let:builder>
            <Button builders={[builder]} size="icon" class="group !bg-transparent">
              <img
                alt={data.session.user.email}
                class="w-full h-full"
                on:mouseenter={() => (hover = true)}
                on:mouseleave={() => (hover = false)}
                src={`https://ui-avatars.com/api/?name=${name}&rounded=true${hover ? '&background=0D8ABC&color=fff' : ''}`} /></Button>
          </DropdownMenu.Trigger>
          <DropdownMenu.Content class="w-56">
            <DropdownMenu.Label>My Account</DropdownMenu.Label>
            <DropdownMenu.Separator />
            <DropdownMenu.Group>
              <DropdownMenu.Item>
                <User class="mr-2 h-4 w-4" />
                <span>Profile</span>
                <DropdownMenu.Shortcut>⇧⌘P</DropdownMenu.Shortcut>
              </DropdownMenu.Item>
              <DropdownMenu.Item>
                <CreditCard class="mr-2 h-4 w-4" />
                <span>Billing</span>
                <DropdownMenu.Shortcut>⌘B</DropdownMenu.Shortcut>
              </DropdownMenu.Item>
              <DropdownMenu.Item>
                <Settings class="mr-2 h-4 w-4" />
                <span>Settings</span>
                <DropdownMenu.Shortcut>⌘S</DropdownMenu.Shortcut>
              </DropdownMenu.Item>
              <DropdownMenu.Item>
                <Keyboard class="mr-2 h-4 w-4" />
                <span>Keyboard shortcuts</span>
                <DropdownMenu.Shortcut>⌘K</DropdownMenu.Shortcut>
              </DropdownMenu.Item>
            </DropdownMenu.Group>
            <DropdownMenu.Separator />
            <DropdownMenu.Group>
              <DropdownMenu.Item>
                <Users class="mr-2 h-4 w-4" />
                <span>Team</span>
              </DropdownMenu.Item>
              <DropdownMenu.Sub>
                <DropdownMenu.SubTrigger>
                  <UserPlus class="mr-2 h-4 w-4" />
                  <span>Invite users</span>
                </DropdownMenu.SubTrigger>
                <DropdownMenu.SubContent>
                  <DropdownMenu.Item>
                    <Mail class="mr-2 h-4 w-4" />
                    <span>Email</span>
                  </DropdownMenu.Item>
                  <DropdownMenu.Item>
                    <MessageSquare class="mr-2 h-4 w-4" />
                    <span>Message</span>
                  </DropdownMenu.Item>
                  <DropdownMenu.Item>
                    <PlusCircle class="mr-2 h-4 w-4" />
                    <span>More...</span>
                  </DropdownMenu.Item>
                </DropdownMenu.SubContent>
              </DropdownMenu.Sub>
              <DropdownMenu.Item>
                <Plus class="mr-2 h-4 w-4" />
                <span>New Team</span>
                <DropdownMenu.Shortcut>⌘+T</DropdownMenu.Shortcut>
              </DropdownMenu.Item>
            </DropdownMenu.Group>
            <DropdownMenu.Separator />
            <DropdownMenu.Item>
              <Github class="mr-2 h-4 w-4" />
              <span>GitHub</span>
            </DropdownMenu.Item>
            <DropdownMenu.Item>
              <LifeBuoy class="mr-2 h-4 w-4" />
              <span>Support</span>
            </DropdownMenu.Item>
            <DropdownMenu.Item>
              <Cloud class="mr-2 h-4 w-4" />
              <span>API</span>
            </DropdownMenu.Item>
            <DropdownMenu.Separator />
            <DropdownMenu.Item on:click={() => (open = true)}>
              <LogOut class="mr-2 h-4 w-4" />
              <span>Log out</span>
              <DropdownMenu.Shortcut>⇧⌘Q</DropdownMenu.Shortcut>
            </DropdownMenu.Item>
          </DropdownMenu.Content>
        </DropdownMenu.Root>
      {/if}
      <nav class="flex items-center">
        <a
          href="https://github.com/huntabyte/shadcn-svelte"
          target="_blank"
          rel="noopener noreferrer"
          ><div
            class="inline-flex items-center justify-center h-8 px-0 text-xs font-medium transition-colors rounded-md whitespace-nowrap focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50 hover:bg-accent hover:text-accent-foreground w-9">
            <svg viewBox="0 0 438.549 438.549" class="w-4 h-4"
              ><path
                fill="currentColor"
                d="M409.132 114.573c-19.608-33.596-46.205-60.194-79.798-79.8-33.598-19.607-70.277-29.408-110.063-29.408-39.781 0-76.472 9.804-110.063 29.408-33.596 19.605-60.192 46.204-79.8 79.8C9.803 148.168 0 184.854 0 224.63c0 47.78 13.94 90.745 41.827 128.906 27.884 38.164 63.906 64.572 108.063 79.227 5.14.954 8.945.283 11.419-1.996 2.475-2.282 3.711-5.14 3.711-8.562 0-.571-.049-5.708-.144-15.417a2549.81 2549.81 0 01-.144-25.406l-6.567 1.136c-4.187.767-9.469 1.092-15.846 1-6.374-.089-12.991-.757-19.842-1.999-6.854-1.231-13.229-4.086-19.13-8.559-5.898-4.473-10.085-10.328-12.56-17.556l-2.855-6.57c-1.903-4.374-4.899-9.233-8.992-14.559-4.093-5.331-8.232-8.945-12.419-10.848l-1.999-1.431c-1.332-.951-2.568-2.098-3.711-3.429-1.142-1.331-1.997-2.663-2.568-3.997-.572-1.335-.098-2.43 1.427-3.289 1.525-.859 4.281-1.276 8.28-1.276l5.708.853c3.807.763 8.516 3.042 14.133 6.851 5.614 3.806 10.229 8.754 13.846 14.842 4.38 7.806 9.657 13.754 15.846 17.847 6.184 4.093 12.419 6.136 18.699 6.136 6.28 0 11.704-.476 16.274-1.423 4.565-.952 8.848-2.383 12.847-4.285 1.713-12.758 6.377-22.559 13.988-29.41-10.848-1.14-20.601-2.857-29.264-5.14-8.658-2.286-17.605-5.996-26.835-11.14-9.235-5.137-16.896-11.516-22.985-19.126-6.09-7.614-11.088-17.61-14.987-29.979-3.901-12.374-5.852-26.648-5.852-42.826 0-23.035 7.52-42.637 22.557-58.817-7.044-17.318-6.379-36.732 1.997-58.24 5.52-1.715 13.706-.428 24.554 3.853 10.85 4.283 18.794 7.952 23.84 10.994 5.046 3.041 9.089 5.618 12.135 7.708 17.705-4.947 35.976-7.421 54.818-7.421s37.117 2.474 54.823 7.421l10.849-6.849c7.419-4.57 16.18-8.758 26.262-12.565 10.088-3.805 17.802-4.853 23.134-3.138 8.562 21.509 9.325 40.922 2.279 58.24 15.036 16.18 22.559 35.787 22.559 58.817 0 16.178-1.958 30.497-5.853 42.966-3.9 12.471-8.941 22.457-15.125 29.979-6.191 7.521-13.901 13.85-23.131 18.986-9.232 5.14-18.182 8.85-26.84 11.136-8.662 2.286-18.415 4.004-29.263 5.146 9.894 8.562 14.842 22.077 14.842 40.539v60.237c0 3.422 1.19 6.279 3.572 8.562 2.379 2.279 6.136 2.95 11.276 1.995 44.163-14.653 80.185-41.062 108.068-79.226 27.88-38.161 41.825-81.126 41.825-128.906-.01-39.771-9.818-76.454-29.414-110.049z"
              ></path
              ></svg> <span class="sr-only" data-svelte-h="svelte-11yow7k">GitHub</span>
          </div></a>
        <a href="https://twitter.com/huntabyte" target="_blank" rel="noreferrer"
          ><div
            class="inline-flex items-center justify-center h-8 px-0 text-xs font-medium transition-colors rounded-md whitespace-nowrap focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50 hover:bg-accent hover:text-accent-foreground w-9">
            <svg
              height="23"
              viewBox="0 0 1200 1227"
              width="23"
              xmlns="http://www.w3.org/2000/svg"
              fill="currentColor"
              class="w-3 h-3 fill-current"
              ><path
                d="M714.163 519.284L1160.89 0H1055.03L667.137 450.887L357.328 0H0L468.492 681.821L0 1226.37H105.866L515.491 750.218L842.672 1226.37H1200L714.137 519.284H714.163ZM569.165 687.828L521.697 619.934L144.011 79.6944H306.615L611.412 515.685L658.88 583.579L1055.08 1150.3H892.476L569.165 687.854V687.828Z"
              ></path
              ></svg>
            <span class="sr-only" data-svelte-h="svelte-13ytjzu"
              >X (formerly known as Twitter)</span>
          </div></a>
        <DropdownMenu.Root>
          <DropdownMenu.Trigger asChild let:builder>
            <Button builders={[builder]} variant="outline" size="icon">
              <Sun
                class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />
              <Moon
                class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />
              <span class="sr-only">Toggle theme</span>
            </Button>
          </DropdownMenu.Trigger>
          <DropdownMenu.Content align="end">
            <DropdownMenu.Item on:click={() => setMode('light')}>Light</DropdownMenu.Item>
            <DropdownMenu.Item on:click={() => setMode('dark')}>Dark</DropdownMenu.Item>
            <DropdownMenu.Item on:click={() => resetMode()}>System</DropdownMenu.Item>
          </DropdownMenu.Content>
        </DropdownMenu.Root>
      </nav>
    </div>
  </div>
</header>
