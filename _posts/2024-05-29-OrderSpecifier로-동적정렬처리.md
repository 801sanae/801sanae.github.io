---
layout: post
title: OrderSpecifier로 동적 정렬 처리
subtitle: QueryDSL Pageable로 동적정렬 Sort 처리
date: 2024-05-29
tags: [JAVA, QueryDSL, PageRequest]
comments: true
---

# QueryDSL에서 동적정렬처리

QueryDSL에서 PageRequest를 이용하여 OrderBy를 처리할 수 있다.

1. PageRequest를 작성한다.
2. PageRequest에서 정렬방향(getDirection), OrderBy할 컬럼(getProperty)을 뽑아서 OrderSpecifier[]로 리턴한다.

```java
//Service Layer
    public List<UserDto> selectAllUser(){
        PageRequest pageRequest = PageRequest.of(0, 30, Sort.Direction.DESC, "seq");
        
        List<UserDto> listUserDto = new ArrayList<>();
        querydslUserRepository.findUsertbl(pageRequest).stream().forEach(u ->{
            listUserDto.add(new UserDto(u));
        });

        return listUserDto;
    }
//Persistence Layer - QueryDSL Query 구성부분
    public List<User> findUsertbl(Pageable pageable){
        return queryFactory
                .selectFrom(user)
                .orderBy(orderSpecifiers(pageable))
                .fetch();
    }

    private OrderSpecifier[] orderSpecifiers(Pageable pageable){
        return pageable.getSort()
                .map(order -> new OrderSpecifier(Order.valueOf(order.getDirection().name()) 
                                    , Expressions.path(User.class, user, order.getProperty())))
                .toList()
                .toArray(new OrderSpecifier[0]);
    }
```