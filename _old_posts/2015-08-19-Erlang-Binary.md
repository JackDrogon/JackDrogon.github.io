---
layout: post
title:  "Erlang Binary"
date:   2015-08-15
lastchang: 2015-08-15
categories: Erlang
---

## BIFS (in stdlib/binary.erl)
1. at/2
2. bin_to_list/1, bin_to_list/2, bin_to_list/3
3. compile_pattern/1
4. copy/1, copy/2
5. decode_unsigned/1, decode_unsigned/2
6. first/1
7. last/1
8. list_to_bin/1
9. longest_common_prefix/1
10. longest_common_suffix
11. match/2 match/3
12. matches/2

```erlang
-export([at/2, bin_to_list/1, bin_to_list/2, bin_to_list/3,
         compile_pattern/1, copy/1, copy/2, decode_unsigned/1,
         decode_unsigned/2, encode_unsigned/1, encode_unsigned/2,
         first/1, last/1, list_to_bin/1, longest_common_prefix/1,
         longest_common_suffix/1, match/2, match/3, matches/2,
 
```

### 1. at/2

at(Subject, Pos), at用来返回binary中的第几个字节，等于把binary看成了一个byte array，此处要注意 Pos >= 0, 超过binary长度的就会throw badarg。  
BTW，这次erlang的下标是从0开始，然而lists，regex等很多其他标准库中很多函数的下标是从1开始的。erlang的标准库的风格让人感觉很不统一，很多时候Subject在后面，这次又在前面。

```erlang
-spec at(Subject, Pos) -> byte() when
      Subject :: binary(),
```
example:

```erlang
binary:at(<<"Hello">>, 0). %% 72 -> "H"
bianry:at(<<"Hello">>, 6). 
%% ** exception error: bad argument
%%     in function  binary:at/2
%%        called as binary:at(<<"Hello">>,6)
```

### 2. bin_to_list/1, bin_to_list/2, bin_to_list/3

bin_to_list(Subject) = bin_to_list(Subject, {0, byte_size(Subject)})
bin_to_list(Subjct, Pos, Len) = bin_to_list(Subject,{Pos,Len})

```erlang
bin_to_list(Subject) -> [byte()]
bin_to_list(Subject, PosLen) -> [byte()], PosLen -> {Pos, Len}
bin_to_list(Subject, Pos, Len) -> [byte()]
```

### 3. compile_pattern/1
### 4. copy/1, copy/2
### 5. decode_unsigned/1, decode_unsigned/2
### 6. first/1
### 7. last/1
### 8. list_to_bin/1
### 9. longest_common_prefix/1
### 10. longest_common_suffix
### 11. match/2 match/3
### 12. matches/2

## Normal Functions

1. split/2, split/3
2. replace/3, replace/4

```erlang
-export([split/2,split/3,replace/3,replace/4]).
```

### 1. split/2, split/3
### 2. replace/3, replace/4


## Binary Utils

```erlang
-module(binary_str).

%%%===================================================================
%%% @doc All return value is number or binary or list of binary, not string
%%%===================================================================

-author('jack.xsuperman@gmail.com').

%% API
-export([equal/2,
         concat/2,
         rchr/2,
         str/2,
         rstr/2,
         span/2,
         cspan/2,
         copies/2,
         words/1,
         words/2,
         sub_word/2,
         sub_word/3,
         strip/1,
         strip/2,
         len/1,
         tokens/2,
         left/2,
         left/3,
         right/2,
         right/3,
         centre/2,
         centre/3,
         sub_string/2,
         sub_string/3,
         to_upper/1,
         join/2,
         substr/2,
         chr/2,
         chars/3,
         chars/2,
         substr/3,
         strip/3,
         to_lower/1,
         to_float/1,
         prefix/2,
         suffix/2,
         to_integer/1]).

%%%===================================================================
%%% API
%%%===================================================================
-spec len(binary()) -> non_neg_integer().

len(B) ->
    byte_size(B).

-spec equal(binary(), binary()) -> boolean().

equal(B1, B2) ->
    B1 == B2.

-spec concat(binary(), binary()) -> binary().

concat(B1, B2) ->
    <<B1/binary, B2/binary>>.

-spec rchr(binary(), char()) -> non_neg_integer().

rchr(B, C) ->
    string:rchr(binary_to_list(B), C).

-spec str(binary(), binary()) -> non_neg_integer().

str(B1, B2) ->
    string:str(binary_to_list(B1), binary_to_list(B2)).

-spec rstr(binary(), binary()) -> non_neg_integer().

rstr(B1, B2) ->
    string:rstr(binary_to_list(B1), binary_to_list(B2)).

-spec span(binary(), binary()) -> non_neg_integer().

span(B1, B2) ->
    string:span(binary_to_list(B1), binary_to_list(B2)).

-spec cspan(binary(), binary()) -> non_neg_integer().

cspan(B1, B2) ->
    string:cspan(binary_to_list(B1), binary_to_list(B2)).

-spec copies(binary(), non_neg_integer()) -> binary().

copies(B, N) ->
    iolist_to_binary(string:copies(binary_to_list(B), N)).

-spec words(binary()) -> pos_integer().

words(B) ->
    string:words(binary_to_list(B)).

-spec words(binary(), char()) -> pos_integer().

words(B, C) ->
    string:words(binary_to_list(B), C).

-spec sub_word(binary(), integer()) -> binary().

sub_word(B, N) ->
    iolist_to_binary(string:sub_word(binary_to_list(B), N)).

-spec sub_word(binary(), integer(), char()) -> binary().

sub_word(B, N, C) ->
    iolist_to_binary(string:sub_word(binary_to_list(B), N, C)).

-spec strip(binary()) -> binary().

strip(B) ->
    iolist_to_binary(string:strip(binary_to_list(B))).

-spec strip(binary(), both | left | right) -> binary().

strip(B, D) ->
    iolist_to_binary(string:strip(binary_to_list(B), D)).

-spec left(binary(), non_neg_integer()) -> binary().

left(B, N) ->
    iolist_to_binary(string:left(binary_to_list(B), N)).

-spec left(binary(), non_neg_integer(), char()) -> binary().

left(B, N, C) ->
    iolist_to_binary(string:left(binary_to_list(B), N, C)).

-spec right(binary(), non_neg_integer()) -> binary().

right(B, N) ->
    iolist_to_binary(string:right(binary_to_list(B), N)).

-spec right(binary(), non_neg_integer(), char()) -> binary().

right(B, N, C) ->
    iolist_to_binary(string:right(binary_to_list(B), N, C)).

-spec centre(binary(), non_neg_integer()) -> binary().

centre(B, N) ->
    iolist_to_binary(string:centre(binary_to_list(B), N)).

-spec centre(binary(), non_neg_integer(), char()) -> binary().

centre(B, N, C) ->
    iolist_to_binary(string:centre(binary_to_list(B), N, C)).

-spec sub_string(binary(), pos_integer()) -> binary().

sub_string(B, N) ->
    iolist_to_binary(string:sub_string(binary_to_list(B), N)).

-spec sub_string(binary(), pos_integer(), pos_integer()) -> binary().

sub_string(B, S, E) ->
    iolist_to_binary(string:sub_string(binary_to_list(B), S, E)).

-spec to_upper(binary()) -> binary();
(char()) -> char().

to_upper(B) when is_binary(B) ->
    iolist_to_binary(string:to_upper(binary_to_list(B)));
to_upper(C) ->
    string:to_upper(C).

-spec join([binary()], binary() | char()) -> binary().

join(L, Sep) ->
    iolist_to_binary(join_s(L, Sep)).

-spec substr(binary(), pos_integer()) -> binary().

substr(B, N) ->
    iolist_to_binary(string:substr(binary_to_list(B), N)).

-spec chr(binary(), char()) -> non_neg_integer().

chr(B, C) ->
    string:chr(binary_to_list(B), C).

-spec chars(char(), non_neg_integer(), binary()) -> binary().

chars(C, N, B) ->
    iolist_to_binary(string:chars(C, N, binary_to_list(B))).

-spec chars(char(), non_neg_integer()) -> binary().

chars(C, N) ->
    iolist_to_binary(string:chars(C, N)).

-spec substr(binary(), pos_integer(), non_neg_integer()) -> binary().

substr(B, S, E) ->
    iolist_to_binary(string:substr(binary_to_list(B), S, E)).

-spec strip(binary(), both | left | right, char()) -> binary().

strip(B, D, C) ->
    iolist_to_binary(string:strip(binary_to_list(B), D, C)).

-spec to_lower(binary()) -> binary();
(char()) -> char().

to_lower(B) when is_binary(B) ->
    iolist_to_binary(string:to_lower(binary_to_list(B)));
to_lower(C) ->
    string:to_lower(C).

-spec tokens(binary(), binary()) -> [binary()].

tokens(B1, B2) ->
    [iolist_to_binary(T) ||
     T <- string:tokens(binary_to_list(B1), binary_to_list(B2))].

-spec to_float(binary()) -> {float(), binary()} | {error, no_float}.

to_float(B) ->
    case string:to_float(binary_to_list(B)) of
        {error, R} ->
            {error, R};
        {Float, Rest} ->
            {Float, iolist_to_binary(Rest)}
    end.

-spec to_integer(binary()) -> {integer(), binary()} | {error, no_integer}.

to_integer(B) ->
    case string:to_integer(binary_to_list(B)) of
        {error, R} ->
            {error, R};
        {Int, Rest} ->
            {Int, iolist_to_binary(Rest)}
    end.

-spec prefix(binary(), binary()) -> boolean().

prefix(Prefix, B) ->
    Size = byte_size(Prefix),
    case B of
        <<Prefix:Size/binary, _/binary>> ->
            true;
        _ ->
            false
    end.

-spec suffix(binary(), binary()) -> boolean().

suffix(B1, B2) ->
    lists:suffix(binary_to_list(B1), binary_to_list(B2)).

%%%===================================================================
%%% Internal functions
%%%===================================================================
join_s([], _Sep) ->
    [];
join_s([H|T], Sep) ->
    [H, [[Sep, X] || X <- T]].

```
