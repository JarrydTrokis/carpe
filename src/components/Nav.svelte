<script lang="ts">
  import { Link, useLocation } from "svelte-navigator";
  import { debugMode } from "../debug";
  import { signingAccount, all_accounts } from "../accounts";
  import AccountSwitcher from "./wallet/AccountSwitcher.svelte";
  import { routes } from "../routes";

  const location = useLocation();
  
  let debug = false;
  debugMode.subscribe((d) => {
    debug = d;
  })

  let myAccountIsOnChain = false;
  signingAccount.subscribe((myAccount) => {
    myAccountIsOnChain = myAccount != null && myAccount.balance != null
  });

  let has_account = false;
  all_accounts.subscribe((list) => {
    has_account = list.length > 0;
  }); 

</script>
<!--
{#if has_account}
        <Nav />
      {:else}
        {#if current.pathname != "/"}
          <p> back </p>
        {/if}
      {/if}
-->

<main class="uk-margin-top">
  <nav class="uk-navbar-container" uk-navbar>
    {#if $location.pathname === routes.settings}
      <Link to={routes.home}><span uk-icon="icon: arrow-left; ratio: 2" /></Link>
    {/if}

    <div class="uk-navbar-center">
      <ul class="uk-navbar-nav">
        {#if myAccountIsOnChain && has_account}
          <li><Link to={routes.home}>Wallet</Link></li>        
          <li><Link to={routes.miner}>Miner</Link></li>
          <li><Link to={routes.transactions}>Transactions</Link></li>
        {/if}

        {#if debug}
          <li><Link to={routes.developer}>Debug</Link></li>
        {/if}
      </ul>
    </div>

    <div class="uk-navbar-right">
      <ul class="uk-navbar-nav">
        <li>
          <AccountSwitcher/>
        </li>
      </ul>
    </div>
  </nav>
</main>
