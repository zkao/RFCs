# musig2-adaptor signature
https://github.com/ElementsProject/scriptless-scripts/blob/ea0a9ea9fbbdfd26f403a84598bcdd92c27a4cc2/md/musig2-adaptorsig.md

This document uses the nomeclature of the reference above, which differs from the rest of RFCs

The correct number of tokens are created for the more generic types and later they get specialized

The "send" transitions transfer information between participants

Upon receiving a msg from bob making a resource available for alice, say bob sends alice his public nounce `RB1`, the party that first creates the type ommits the the ownership tag, as such bob's type is `RB1` while the type encoding that alice knows `RB1` is `RB1_a`.

regarding `presign`, the `pres_a` type encodes pre-signature of alice, and `pres_a_b` means bob knowns pres_a, and `pres_ab_b` means that bob knows `pres_a_b` and `pres_b`, and can construct the adapted multisignature that requires `t` to become valid.

``` net:alice
keygen_a () -> a A A A A
setup_sess_a () -> rA1 rA2 RA1 RA1 RA1 RA1 RA1 RA2 RA2 RA2 RA2 RA2

presign_a a B_a rA1 RA1 rA2 RA2 RB1_a RB2_a m T_a -> pres_a pres_a pres_a
preverify_a B_a RB1 RB2 A RA1 RA2 m T_a pres_b_a -> VerifiedA

msg A RA1 RA2 B_a RB1_a RB2_a T_a  -> m m m m

aggreg_a VerifiedA pres_a pres_b_a -> pres_ab_a
send0a RA1 RA2 A A pres_a pres_a m m -> RA1_b RA2_b A_b A_b pres_a_b pres_a_b m_b m_b
ext_a s pres_ab_a -> t_a
```

``` net:bob
adaptor_gen () -> t T T T T T

keygen_b () -> b B B B B
setup_sess_b () -> rB1 rB2 RB1 RB1 RB1 RB1 RB1 RB2 RB2 RB2 RB2 RB2

presign_b b A_b rB1 RB1 rB2 RB2 RA1_b RA2_b m_b T -> pres_b pres_b pres_b
preverify_b B RB1 RB2 A_b RA1 RA2 m_b T pres_a_b -> VerifiedB

aggreg_b VerifiedB pres_a_b pres_b -> pres_ab_b

send0b RB1 RB1 RB2 RB2 T T T B B B -> RB1_a RB1_a RB2_a RB2_a T_a T_a T_a B_a B_a B_a
send1b pres_b pres_b -> pres_b_a pres_b_a
adapt_b pres_ab_b t -> s
```
