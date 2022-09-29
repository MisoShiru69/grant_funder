resim reset

export XRD=resource_sim1qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqzqu57yag

OP1=$(resim new-account)
export PRIV_KEY1=$(echo "$OP1" | sed -nr "s/Private key: ([[:alnum:]_]+)/\1/p")
export PUB_KEY1=$(echo "$OP1" | sed -nr "s/Public key: ([[:alnum:]_]+)/\1/p")
export ACCOUNT_ADDRESS1=$(echo "$OP1" | sed -nr "s/Account component address: ([[:alnum:]_]+)/\1/p")

PK_OP=$(resim publish ".")
echo $PK_OP
export PACKAGE=$(echo "$PK_OP" | sed -nr "s/Success! New Package: ([[:alnum:]_]+)/\1/p")
echo $PACKAGE

COMPONENT_OP=$(resim call-function $PACKAGE GrantFunder new 700,$XRD)
echo $COMPONENT_OP

export COMPONENT=$(echo "$COMPONENT_OP" | sed -nr "s/.* Component: ([[:alnum:]_]+)/\1/p" | sed '1q;d')

export PROPOSAL_RECEIPT=$(echo "$COMPONENT_OP" | sed -nr "s/.* Resource: ([[:alnum:]_]+)/\1/p" | sed '3q;d')

resim call-method $COMPONENT create_proposal first 100
resim call-method $COMPONENT vote_yes 1,$XRD
resim call-method $COMPONENT vote_yes 1,$XRD
resim call-method $COMPONENT vote_yes 1,$XRD
resim call-method $COMPONENT vote_yes 1,$XRD
resim call-method $COMPONENT vote_yes 1,$XRD
resim call-method $COMPONENT vote_no 1,$XRD
resim call-method $COMPONENT vote_no 1,$XRD
resim call-method $COMPONENT vote_no 1,$XRD

resim show $COMPONENT

resim call-method $COMPONENT count_votes 300710000000f4e391868046a709954c6ba380933d26

resim call-method $COMPONENT update_receipt 1,$PROPOSAL_RECEIPT 

resim call-method $COMPONENT collect_xrd 1,$PROPOSAL_RECEIPT

resim call-method $COMPONENT count_votes 300710000000f4e391868046a709954c6ba380933d26

resim call-method $COMPONENT update_receipt 1,$PROPOSAL_RECEIPT

resim call-method $COMPONENT collect_xrd 1,$PROPOSAL_RECEIPT
