{% comment %}
This file is licensed under the MIT License (MIT) available on
http://opensource.org/licenses/MIT.
{% endcomment %}
{% assign filename="_includes/devdoc/dash-core/rpcs/rpcs/listunspent.md" %}

##### ListUnspent
{% include helpers/subhead-links.md %}

{% assign summary_listUnspent="returns an array of unspent transaction outputs belonging to this wallet." %}

<!-- __ -->

{% autocrossref %}

*Requires wallet support.*

The `listunspent` RPC {{summary_listUnspent}} **Note:** as of Bitcoin
Core 0.10.0, outputs affecting watch-only addresses will be returned; see
the *spendable* field in the results described below.

*Parameter #1---the minimum number of confirmations an output must have*

{% itemplate ntpd1 %}
- n: "Minimum Confirmations"
  t: "number (int)"
  p: "Optional<br>(0 or 1)"
  d: "The minimum number of confirmations the transaction containing an output must have in order to be returned.  Use `0` to return outputs from unconfirmed transactions. Default is `1`"

{% enditemplate %}

*Parameter #2---the maximum number of confirmations an output may have*

{% itemplate ntpd1 %}
- n: "Maximum Confirmations"
  t: "number (int)"
  p: "Optional<br>(0 or 1)"
  d: "The maximum number of confirmations the transaction containing an output may have in order to be returned.  Default is `9999999` (~10 million)"

{% enditemplate %}

*Parameter #3---the addresses an output must pay*

{% itemplate ntpd1 %}
- n: "Addresses"
  t: "array"
  p: "Optional<br>(0 or 1)"
  d: "If present, only outputs which pay an address in this array will be returned"

- n: "→<br>Address"
  t: "string (base58)"
  p: "Required<br>(1 or more)"
  d: "A P2PKH or P2SH address"

{% enditemplate %}

*Result---the list of unspent outputs*

{% itemplate ntpd1 %}
- n: "`result`"
  t: "array"
  p: "Required<br>(exactly 1)"
  d: "An array of objects each describing an unspent output.  May be empty"

- n: "→<br>Unspent Output"
  t: "object"
  p: "Optional<br>(0 or more)"
  d: "An object describing a particular unspent output belonging to this wallet"

- n: "→ →<br>`txid`"
  t: "string (hex)"
  p: "Required<br>(exactly 1)"
  d: "The TXID of the transaction containing the output, encoded as hex in RPC byte order"

- n: "→ →<br>`vout`"
  t: "number (int)"
  p: "Required<br>(exactly 1)"
  d: "The output index number (vout) of the output within its containing transaction"

- n: "→ →<br>`address`"
  t: "string (base58)"
  p: "Optional<br>(0 or 1)"
  d: "The P2PKH or P2SH address the output paid.  Only returned for P2PKH or P2SH output scripts"

- n: "→ →<br>`account`"
  t: "string"
  p: "Optional<br>(0 or 1)"
  d: "*Deprecated: will be removed in a later version of Dash Core*<br><br>If the address returned belongs to an account, this is the account.  Otherwise not returned"

- n: "→ →<br>`scriptPubKey`"
  t: "string (hex)"
  p: "Required<br>(exactly 1)"
  d: "The output script paid, encoded as hex"

- n: "→ →<br>`redeemScript`"
  t: "string (hex)"
  p: "Optional<br>(0 or 1)"
  d: "If the output is a P2SH whose script belongs to this wallet, this is the redeem script"

- n: "→ →<br>`amount`"
  t: "number (int)"
  p: "Required<br>(exactly 1)"
  d: "The amount paid to the output in dash"

- n: "→ →<br>`confirmations`"
  t: "number (int)"
  p: "Required<br>(exactly 1)"
  d: "The number of confirmations received for the transaction containing this output"

- n: "→ →<br>`ps_rounds`"
  t: "number (int)"
  p: "Required<br>(exactly 1)"
  d: "The number of PrivateSend rounds"

- n: "→ →<br>`spendable`"
  t: "bool"
  p: "Required<br>(exactly 1)"
  d: "Set to `true` if the private key or keys needed to spend this output are part of the wallet.  Set to `false` if not (such as for watch-only addresses)"

- n: "→ →<br>`solvable`"
  t: "bool"
  p: "Required<br>(exactly 1)"
  d: "*Added in Bitcoin Core 0.13.0*<br><br>Set to `true` if the wallet knows how to spend this output.  Set to `false` if the wallet does not know how to spend the output.  It is ignored if the private keys are available "

{% enditemplate %}

*Example from Dash Core 0.12.2*

Get all outputs confirmed at least 6 times for a particular
address:

{% highlight bash %}
dash-cli -testnet listunspent 6 99999999 '''
  [
    "yTQNnbby2yhxoK1UtL9E5J9epGtkAoFWSm"
  ]
'''
{% endhighlight %}

Result:

{% highlight json %}
[
  {
    "txid": "52e34eec71a4cf95c043b76567f55cec1bc293c444810d454a2d05f2a819b5ed",
    "vout": 3,
    "address": "yTQNnbby2yhxoK1UtL9E5J9epGtkAoFWSm",
    "scriptPubKey": "76a9144db791c2388be4716f048be2648bafe1944f787688ac",
    "amount": 0.01000010,
    "confirmations": 113,
    "ps_rounds": 4,
    "spendable": true,
    "solvable": true
  }
]
{% endhighlight %}

*See also*

* [ListTransactions][rpc listtransactions]: {{summary_listTransactions}}
* [LockUnspent][rpc lockunspent]: {{summary_lockUnspent}}

{% endautocrossref %}
