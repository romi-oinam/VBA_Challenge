# VBA Challenge

Sub Stock_Analysis():


    Dim Ticker As String

    Dim open_price As Double

    Dim close_price As Double

    Dim Yearly_Change As Double
    
    Dim Percent_Change As Double

    Dim Total_Stock_Volume As Double

    Dim summaryRow As Integer

    Dim ws As Worksheet


For Each ws In Worksheets


    ws.Cells(1, 9).Value = "Ticker"
    ws.Cells(1, 10).Value = "Yearly Change"
    ws.Cells(1, 11).Value = "Percent Change"
    ws.Cells(1, 12).Value = "Total Stock Volume"

    
    summaryRow = 2
    previous_i = 1
    Total_Stock_Volume = 0

    LastRow = ws.Cells(Rows.Count, "A").End(xlUp).Row


        For i = 2 To LastRow


            If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then

            Ticker = ws.Cells(i, 1).Value

            previous_i = previous_i + 1

           

            open_price = ws.Cells(previous_i, 3).Value
            close_price = ws.Cells(i, 6).Value

           

        For j = previous_i To i

            Total_Stock_Volume = Total_Stock_Volume + ws.Cells(j, 7).Value

            Next j

            

            If open_price = 0 Then

                Percent_Change = close_price

            Else
                Yearly_Change = close_price - open_price

                Percent_Change = Yearly_Change / open_price

            End If
         

            ws.Cells(summaryRow, 9).Value = Ticker
            ws.Cells(summaryRow, 10).Value = Yearly_Change
            ws.Cells(summaryRow, 11).Value = Percent_Change


            ws.Cells(summaryRow, 11).NumberFormat = "0.00%"
            ws.Cells(summaryRow, 12).Value = Total_Stock_Volume


            summaryRow = summaryRow + 1

        
            Total_Stock_Volume = 0
            Yearly_Change = 0
            Percent_Change = 0

            previous_i = i

        End If


    Next i

  jLastRow = ws.Cells(Rows.Count, "J").End(xlUp).Row


        For j = 2 To jLastRow

            If ws.Cells(j, 10) > 0 Then

                ws.Cells(j, 10).Interior.ColorIndex = 4

            Else

                ws.Cells(j, 10).Interior.ColorIndex = 3
            End If

        Next j



    kLastRow = ws.Cells(Rows.Count, "K").End(xlUp).Row

    Increase = 0
    Decrease = 0
    Greatest = 0

        
        For k = 3 To kLastRow

            last_k = k - 1

            current_k = ws.Cells(k, 11).Value

            prevous_k = ws.Cells(last_k, 11).Value

            volume = ws.Cells(k, 12).Value

            prevous_vol = ws.Cells(last_k, 12).Value

        If Increase > current_k And Increase > prevous_k Then

                Increase = Increase

        ElseIf current_k > Increase And current_k > prevous_k Then

                Increase = current_k

                increase_name = ws.Cells(k, 9).Value

        ElseIf prevous_k > Increase And prevous_k > current_k Then

                Increase = prevous_k

                increase_name = ws.Cells(last_k, 9).Value

            End If


        If Decrease < current_k And Decrease < prevous_k Then


                Decrease = Decrease


        ElseIf current_k < Increase And current_k < prevous_k Then

                Decrease = current_k


                decrease_name = ws.Cells(k, 9).Value

            ElseIf prevous_k < Increase And prevous_k < current_k Then

                Decrease = prevous_k

                decrease_name = ws.Cells(last_k, 9).Value

            End If


            If Greatest > volume And Greatest > prevous_vol Then

                Greatest = Greatest


            ElseIf volume > Greatest And volume > prevous_vol Then

                Greatest = volume

                greatest_name = ws.Cells(k, 9).Value

            ElseIf prevous_vol > Greatest And prevous_vol > volume Then

                Greatest = prevous_vol

                greatest_name = ws.Cells(last_k, 9).Value

            End If

        Next k
 
    ws.Range("N2").Value = "Greatest % Increase"
    ws.Range("N3").Value = "Greatest % Decrease"
    ws.Range("N4").Value = "Greatest Total Volume"
    ws.Range("O1").Value = "Ticker Name"
    ws.Range("P1").Value = "Value"

    ws.Range("O2").Value = increase_name
    ws.Range("O3").Value = decrease_name
    ws.Range("O4").Value = greatest_name
    ws.Range("P2").Value = Increase
    ws.Range("P2").NumberFormat = "0.00%"
    
    ws.Range("P3").Value = Decrease
    ws.Range("P3").NumberFormat = "0.00%"
    
    ws.Range("P4").Value = Greatest

  
Next ws

End Sub

