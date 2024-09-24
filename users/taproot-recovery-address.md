# Taproot Recovery Address

Taproot expands Bitcoin’s programmability by allowing the creation of if-else conditions, which can also be called spending conditions. Citrea’s Taproot contract has two spending conditions that are related to deposits. 


The first spending condition is if the verifiers accept the deposit. The bridge verifiers accept the deposit only if it equals a predefined fixed amount. This is a limitation of BitVM-based bridges, though we are working and researching on better UX with batch deposits and batch withdrawals. If the verifiers accept the deposit, the bridge operator takes the deposit and mints cBTC on Citrea and sends it to the EVM address provided by the user.


In the second spending condition is if the verifiers do not accept the deposit, which will be the case if the deposit does not equal to the predefined amount. Then, the user gets the deposit back 200 Bitcoin blocks later. The Recovery Taproot address is used for the second spending condition to allow users to trustlessly claim their deposit from the taproot address specified on the bridge page. This specified taproot address is either randomly generated on the Citrea bridge page or is provided by the user.


The current Citrea Testnet fixed amount of deposits/withdrawals are subject to change on the Citrea mainnet.

