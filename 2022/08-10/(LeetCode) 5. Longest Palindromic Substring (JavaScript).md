# 문제

Given a string s, return the longest palindromic substring in s.

문자열 s가 주어졌을때 s의 substring 중 제일 긴 팰린드롬 문자를 찾아라

🤡 **팰린드롬(Palindromic)이란?**

> 거꾸로 읽어도 제대로 읽는 것과 같은 문장이나 낱말, 숫자, 문자열(sequence of characters) 등을 말한다.

# 풀이

## 팰린드롭 찾기 함수

팰린드롭은 길이가 짝수인 경우와 홀수인 경우로 나뉜다.

짝수인 경우
```plaintext
abba
aa
abccba
...
```

홀수인 경우
```plaintext
aba
a
abcba
...
```

```javascript
/**
 * 팰린드롭 문자열 찾기 함수
 * 
 * @param {string} s - Palindrome 검사할 문자열
 * @param {string} i - Palindrome 검사할 문자열의 시작 인덱스
 * @param {string} j - Palindrome 검사할 문자열의 끝 인덱스
 * 
 * @return {string} - 가장 긴 팰린드롭 문자열
 */
var findPalindrome = function(s, i, j) {
    while (i >= 0 && j < s.length && s[i] === s[j]) {
        i--;
        j++;
    }
    return s.slice(i + 1, j);
}
```

1. i, j 값을 기준으로 좌(i)우(j)로 1씩 이동하며 팰린드롭 검사를 한다.
2. 조건
    -  `i >= 0` 0보다 작지 않을 떄까지 좌로 이동
    -  `j < s.length` 문자열 s보다 크지 않을 떄까지 우로 이동
    - `s[i] === s[j]` 좌우로 1씩 이동 시 해당 문자열 값이 같아야 팰린드롬에 해당한다.


## 전체 소스 코드

```javascript
/**
 * Given a string s, return the longest palindromic substring in s
 * 
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    if (s.length === 0) return '';
    let longest = '';
    for (let i = 0; i < s.length; i++) {
        let odd = findPalindrome(s, i, i);
        let even = findPalindrome(s, i, i + 1);
        longest = odd.length > even.length ? odd : even;
    }
    return longest;
};

var findPalindrome = function(s, i, j) {
    while (i >= 0 && j < s.length && s[i] === s[j]) {
        i--;
        j++;
    }
    return s.slice(i + 1, j);
}

```