<script>
  import { navigate } from "svelte-navigator";
  import Input from "../../components/Form/Input.svelte";
  import PasswordInput from "../../components/Form/PasswordInput.svelte";
  import { Translate,TranslateApiMessage } from "../../services/language";
  import RestService from "../../services/rest";
  import { email,token } from "../../services/store";
  import ToastService from "../../services/toast";
    let emailFromInput;
  let password;

  let emailFromInputIsValid;
  let passwordIsValid;

  const login = async () => {
    let response = await RestService.login(emailFromInput, password);
    if (response && response.status) {
      email.set(response.email);
      token.set(response.token);
      navigate("/admin");
      ToastService.success("İşlem başarılı");
    } else {
      console.log("eerrro", response);
      ToastService.error($TranslateApiMessage(response.message));
    }
  };
</script>

<div class="container mx-auto px-4 h-full">
  <div class="flex content-center items-center justify-center h-full">
    <div class="w-full lg:w-4/12 px-4">
      <div
        class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-blueGray-200 border-0"
      >
        <div class="rounded-t mb-0 px-6 py-6">
          <div class="text-center mb-3">
            <h6 class="text-blueGray-500 text-sm font-bold">
              {$Translate("Log-In")}
            </h6>
          </div>
          <!-- <div class="btn-wrapper text-center">
              <button
                class="bg-white active:bg-blueGray-50 text-blueGray-700 font-normal px-4 py-2 rounded outline-none focus:outline-none mr-2 mb-1 uppercase shadow hover:shadow-md inline-flex items-center font-bold text-xs ease-linear transition-all duration-150"
                type="button"
              >
                <img alt="..." class="w-5 mr-1" src="/assets/img/github.svg" />
                Github
              </button>
              <button
                class="bg-white active:bg-blueGray-50 text-blueGray-700 font-normal px-4 py-2 rounded outline-none focus:outline-none mr-1 mb-1 uppercase shadow hover:shadow-md inline-flex items-center font-bold text-xs ease-linear transition-all duration-150"
                type="button"
              >
                <img alt="..." class="w-5 mr-1" src="/assets/img/google.svg" />
                Google
              </button>
            </div> -->
          <hr class="mt-6 border-b-1 border-blueGray-300" />
        </div>
        <div class="flex-auto px-4 lg:px-10 py-10 pt-0">
          <form>
            <div class="relative w-full mb-3">
              <label
                class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                for="grid-email"
              >
                {$Translate("Email")}
              </label>

              <Input
                bind:value={emailFromInput}
                bind:isValid={emailFromInputIsValid}
                placeholder={$Translate("Your-email")}
                required={true}
                type={"email"}
              />
            </div>

            <div class="relative w-full mb-3">
              <label
                class="block uppercase text-blueGray-600 text-xs font-bold mb-2"
                for="grid-password"
              >
                {$Translate("Password")}
              </label>

              <PasswordInput
                bind:value={password}
                bind:isValid={passwordIsValid}
                placeholder={$Translate("Your-password")}
                required={true}
              />
            </div>
            <div>
              <label class="inline-flex items-center cursor-pointer">
                <input
                  id="customCheckLogin"
                  type="checkbox"
                  class="form-checkbox border-0 rounded text-blueGray-700 ml-1 w-5 h-5 ease-linear transition-all duration-150"
                />
                <span class="ml-2 text-sm font-semibold text-blueGray-600">
                  {$Translate("Remember-me")}
                </span>
              </label>
            </div>

            <div class="text-center mt-6">
              <button
                on:click|preventDefault={login}
                disabled={!emailFromInputIsValid || !passwordIsValid
                  ? true
                  : false}
                class="disabled:bg-blueGray-400 bg-blueGray-800 text-white active:bg-blueGray-600 text-sm font-bold uppercase px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 w-full ease-linear transition-all duration-150"
                type="button"
              >
                {$Translate("Log-In")}
              </button>
            </div>
          </form>
        </div>
      </div>
      <div class="flex flex-wrap mt-6 relative">
        <div class="w-1/2">
          <a
            href="#pablo"
            on:click={(e) => e.preventDefault()}
            class="text-blueGray-200"
          >
            <small> {$Translate("Forgot-password")}</small>
          </a>
        </div>
        <!-- <div class="w-1/2 text-right">
            <a use:link href="/auth/register" class="text-blueGray-200">
              <small>Create new account</small>
            </a>
          </div> -->
      </div>
    </div>
  </div>
</div>
