# EmployeeDetails
Putting in Json Power DB
<!DOCTYPE html>
<!--
To change this license header, choose License Headers in Project Properties.
To change this template file, choose Tools | Templates
and open the template in the editor.
-->
<html>
    <head>
        <title>Employee Details</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
    </head>

    <body>
        
            <h2> Employee Form </h2>
            <form id="empform" method="post">
                <div>
                    <label for="empId" >Employee ID: </label>
                    <input type="text" placeholder="Enter Employee ID"  id="empID" required>
                    <br>
                </div>
                <div>
                    <label for="empname" id="empname">Employee Name: </label>
                    <input type="text"  id="empname" placeholder="Your Full Name" required>
                    <br>  
                </div>
                <div>
                    <label for="basic" id="basic">Basic Salary: </label>
                    <input type="text" id="basic" >
                    <br>
                </div>
                <div>
                    <label id="hra"> HRA: </label>
                    <input type="text"  id="hra">
                    <br>
                </div>
                <div>
                    <label for="da" id="da">DA: </label>
                    <input type="text"  id="da">
                    <br>
                </div>
                <div>
                    <label for="ded" id="ded">Deductions: </label>
                    <input type="text"  id="ded">
                    <br>
                </div>
                <input type="button" value=" Save " id="saveemp" onclick="saveEmployee();">
            </form>    
        

        <script>
            $("#empId").focus();


            function saveEmployee() {
                var jsonStr = validateAndGetFormData();
                if (jsonStr === "") {
                    return;
                }

                var putReqStr = createPUTRequest("90935784|-31948838670690820|90934714", jsonStr, "Employee", "root");
                alert(putReqStr);
                jQuery.ajaxSetup({async: false});
                var resultObj = executeCommand(putReqStr, "http://api.login2explore.com:5577", "/api/iml");
                alert(JSON.stringify(resultObj));
                jQuery.ajxSetup({async: true});

                resetForm();
            }

            function validateAndGetFormData() {
                var empIdVar = $("#empId").val();
                if (empIdVar === "") {
                    alert("Employee ID Required");
                    $("#empId").focus();
                    return "";
                }

                var empNameVar = $("#empname").val();
                if (empName === "") {
                    alert("Employee Name Required");
                    $("#empname").focus();
                    return "";
                }

                var basicVar = $("#basic").val();
                if (basic === "") {
                    alert("Basic Sal required");
                    $("#basic").focus();
                    return "";
                }

                var hraVar = $("#hra").val();
                if (hra === "") {
                    alert("HRA Required");
                    $("#hra").focus();
                    return "";
                }

                var daVar = $("#da").val();
                if (da === "") {
                    alert("DA Required");
                    $("#da").focus();
                    return "";
                }

                var dedVar = $("#ded").val();
                if (ded === "") {
                    alert("Deductions Required");
                    $("#ded").focus();
                    return "";
                }

                var jsonStrObj = {
                    empId: empIdVar,
                    empname: empNameVar,
                    basic: basicVar,
                    hra: hraVar,
                    da: daVar,
                    ded: dedVar
                };

                return JSON.stringify(jsonStrObj);
            }

            function createPUTRequest(connToken, jsonObj, dbName, relName) {
                var putRequest = "{\n"
                        + "\"token\" : \""
                        + connToken
                        + "\","
                        + "\"dbName\": \""
                        + dbName
                        + "\",\n" + "\"cmd\" : \"PUT\",\n"
                        + "\"rel\" : \""
                        + relName + "\","
                        + "\"jsonStr\": \n"
                        + jsonObj
                        + "\n"
                        + "}";
                return putRequest;
            }

            function executeCommand(reqString, dbBaseUrl, apiEndPointUrl) {
                var url = dbBaseUrl + apiEndPointUrl;
                var jsonObj;
                $.post(url, reqString, function (result) {
                    jsonObj = JSON.parse(result);
                }).fail(function (result) {
                    var dataJsonObj = result.responseText;
                    jsonObj = JSON.parse(dataJsonObj);
                });
                return jsonObj;
            }
            
            function resetForm(){
                $("#empId").val("");
                $("#empname").val("");
                $("#basic").val("");
                $("#hra").val("");
                $("#da").val("");
                $("#ded").val("");
                
            }

        </script>
    </body>
</html>
