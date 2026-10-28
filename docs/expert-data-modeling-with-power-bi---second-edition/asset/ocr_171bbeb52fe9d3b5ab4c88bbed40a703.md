| Numeric | 0 | Displays a digit or zero. If the number has fewer digits as the number of zeros in the format string, it puts leading or trailing zeros. If the number has more digits, it rounds the number to as the number of zeros in the format string. | FORMAT(0, "0,0") returns 00
FORMAT(0.1153, "0.00") returns 0.12
FORMAT(35.1113, "0,00.00") returns 035.11 |