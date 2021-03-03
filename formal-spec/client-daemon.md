## Atomic Data (Daemon to Client)

### Keys
- `0x01`: `Ab` Alice buy key
- `0x02`: `Ac` Alice cancel key
- `0x03`: `Ar` Alice refund key
- `0x04`: `Ap` Alice punish key
- `0x05`: `Ta` Alice adaptor key
- `0x06`: `Ksa` -> K_s^a Alice spend key
- `0x07`: `Kva` -> K_v^a Alice private view key
- `0x08`: `Bf` Bob fund key
- `0x09`: `Bb` Bob buy key
- `0x0a`: `Bc` Bob cancel key
- `0x0b`: `Br` Bob refund key
- `0x0c`: `Tb` Bob adaptor key
- `0x0d`: `Ksb` -> K_s^b Bob spend key
- `0x0e`: `Kvb` -> K_v^b Bob private view key

### Transactions
- `0x01`: `funding` (a) arbitrating transaction
- `0x02`: `lock` (b) arbitrating transaction
- `0x03`: `buy` (c) arbitrating transaction
- `0x04`: `cancel` (d) arbitrating transaction
- `0x05`: `refund` (e) arbitrating transaction
- `0x06`: `punish` (f) arbitrating transaction

## The alice_session_params Bundle
- data:
  - The `buy` `Ab` public key
  - The `cancel` `Ac` public key
  - The `refund` `Ar` public key
  - The `punish` `Ap` public key
  - The `Ta` adaptor public key
  - The `AAddress` destination Bitcoin address
  - The `Kva` -> K_v^a view private key
  - The `Ksa` -> K_s^a spend public key
  - The `za` cross-group discrete logarithm zero-knowledge proof
  - The cancel `t0` and punish `t1` timelocks
  - The `fee` fee strategy

From Alice's Client to Daemon:

``` net:a_client
params_a Ab Ac Ar Ap Ta AAddress Kva Ksa za t0 t1 fee -> ASessionsParams
```

``` net:a_daemon
decode ASessionsParams -> Ab Ac Ar Ap Ta AAddress Kva Ksa za t0 t1 fee
digest Ab Ac Ar Ap Ta kva Ksa -> ACommit
commit_a ACommit -> ACommit_b 
; Wait for bob to commit as well
reveal_a Ab Ac Ar Ap Ta kva Ksa -> AReveal_b
```



After Daemon confirmed parameters match the previously commited ones.

from Alice Daemon to Client:
```net:bob_session_params_a
encode_da Ab_db Ac_db Ar_db Ap_db Ta_db AAddress_db Kva_db Ksa_db za_db t0_db t1_db fee_db -> Msg1da
send1_da Msg1da  -> Msg1ca

```

## The bob_session_params Bundle
- data:
  - The buy `Bb` public key
  - The cancel `Bc` public key
  - The refund `Br` public key
  - The `Tb` adaptor public key
  - The `BAddress` refund Bitcoin address
  - The `Kvb` -> K_v^b view private key
  - The `Ksb` -> K_s^b spend public key
  - The `zb` cross-group discrete logarithm zero-knowledge proof
  - The cancel `t0` and punish `t1` timelocks
  - The `fee` fee strategy


from Bob's Client to Daemon:
``` net:bob_session_params
send0cd_b Bb Bc Br Tb BAddress Kvb Ksb zb t0 t1 fee -> Bb_db Bc_db Br_db Tb_db BAddress_db Kvb_db Ksb_db zb_db t0_db t1_db fee_db
```

from Bob's Daemon to Client:
```net:alice_session_params_b
send1dc_b Ab_db Ac_db Ar_db Ap_db Ta_db AAddress_db Kva_db Ksa_db za_db t0_db t1_db fee_db -> Ab_cb Ac_cb Ar_cb Ap_cb Ta_cb AAddress_cb Kva_cb Ksa_cb za_cb t0_cb t1_cb fee_cb
```
## The cosigned_arbitrating_cancel Bundle
- data:
  - The `ab_canclsig` -> Ac|Bc cancel (d) signature

from Bob's Client to Daemon
``` net:cosigned_arbitrating_cancel
send1cd_b ab_canclsig -> ab_canclsig_d

```

FIXME: is the following needed? Alice should be able 
form Alice Daemon to Client
## The core_arbitrating_transactions Bundle
- data:
  - The `lock` (b) transaction
  - The `cancel` (d) transaction
  - The `refund` (e) transaction



## The signed_adaptor_buy Bundle
- data:
  - The Bb(Ta) buy (c) adaptor signature

## The fully_signed_buy Bundle
- data:
  - The Ab buy (c) signature
  - The adapted Bb(Ta) buy (c) adaptor signature

## The signed_adaptor_refund Bundle
- data:
  - The Ar(Tb) refund (e) adaptor signature

## The fully_signed_refund Bundle
- data:
  - The Br refund (e) signature
  - The adapted Ar(Tb) refund (e) adaptor signature

## The signed_arbitrating_lock Bundle
- data:
  - The Bf lock (b) signature for unlocking the funding

## The signed_arbitrating_punish Bundle
- data:
  - The Ap punish (f) signature for unlocking the cancel transaction UTXO
