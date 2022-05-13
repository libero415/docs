비거주자양도차익 

- 양식 :엑셀서식

(탭구분)
USER_ID	YEAR	MONTH	TRANSACTION_TYPE	BASE_ASSET_NAME	QUOTE_ASSET_NAME	BASE_AMOUNT	QUOTE_AMOUNT	PRICE	판매금액(원화환산)_과세	판매금액(원화환산)_비과세	판매수량중_과세수량	판매수량중_비과세수량	판매수량중_과세취득금액	판매수량중_비과세취득금액	양도차익	양도차익잔액	과세출금액	비과세출금액	원화잔고	가상자산잔고	가상자산가액잔고

(, 구분)
USER_ID,YEAR,MONTH,TRANSACTION_TYPE,BASE_ASSET_NAME,QUOTE_ASSET_NAME,BASE_AMOUNT,QUOTE_AMOUNT,PRICE,판매금액(원화환산)_과세,판매금액(원화환산)_비과세,판매수량중_과세수량,판매수량중_비과세수량,판매수량중_과세취득금액,판매수량중_비과세취득금액,양도차익,양도차익잔액,과세출금액,비과세출금액,원화잔고,가상자산잔고,가상자산가액잔고

- 대상
외국인회원 목록 (기 제출 자료)

SELECT

	  aa.ACCT_ID AS "회원번호"
	, CASE 
	   WHEN aa.ACCT_STAT = 9 THEN '탈퇴' 
   	   WHEN aa.ACCT_STAT = 2 THEN '휴면' 
   	   WHEN aa.ACCT_STAT = 1 THEN '정상'
          END AS "계정상태"
	, aa.FORN_YN AS "외국인회원여부"
		
FROM (	

	SELECT
	
		  a.ACCT_ID
		, a.ACCT_STAT
	   , CASE 
			WHEN a.ACCT_STAT = 2 THEN b.FORN_YN
			WHEN a.ACCT_STAT = 9 THEN c.FORN_YN
		  ELSE a.FORN_YN END AS "FORN_YN"
		
	FROM ledger.tbl_acct_info a 
	LEFT OUTER JOIN dormant.tbl_acct_dm_info b ON a.ACCT_ID = b.ACCT_ID -- 휴면
	LEFT OUTER JOIN dormant.tbl_acct_wd_info c ON a.ACCT_ID = c.ACCT_ID -- 탈퇴
	
) aa
WHERE aa.FORN_YN = 'Y' -- 외국인회원 
;

