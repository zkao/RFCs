``` net:commit_alice_session_params_
send0a a_params_digest -> Sent0a a_params_digest_b 
```

``` net:commit_bob_session_params_
send0b b_params_digest -> Sent0b b_params_digest_a
```

``` net:reveal_alice_session_params_
send1a  Sent0a Sent0b kva Ksa Ab Ta Ac Ar Ap AAddress za  -> Sent1a kva_b Ksa_b Ab_b Ta_b Ac_b Ar_b Ap_b AAddress_b za_b
```

``` net:reveal_bob_session_params_
send1b Sent0a Sent0b kbv Ksb Bb Tb Bc zb Br BAddress -> Sent1b kbv_a Ksb_a Bb_a Tb_a Bc_a zb_a Br_a BAddress_a
```

``` net:core_arbitrating_setup_
send2b Sent1b OKsend2b lock cancel refund b_sig_cancel -> Sent2b lock_a cancel_a refund_a b_sig_cancel_a
```

``` net:refund_procedure_signatures_
send2a Sent1a Sent2b OKsend2a a_sig_cancel refund_adaptor_sig -> Sent2a a_sig_cancel_b refund_adaptor_sig_b
```


``` net:buy_procedure_signature_
send3b Sent2b OKsend3b buy buy_adaptor_sig -> Sent3b buy_a buy_adaptor_sig_a
```
