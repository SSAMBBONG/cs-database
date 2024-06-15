# 정규화

## 3NF

모든 non-prime attribute는 어떤 key에도 transitively dependent 하면 안된다.

non-prime attribute와 non-prime attribute 사이에는 fd가 있으면 안된다.

## BCNF

모든 유효한 non-travial FD x -> y 는 x가 super key여야 한다.

## denormalization

성능을 고려햐여 정규화 반대과정을 진행하는 것
