---
path: "/docs/managers/cics-ts-ceci-manager"
title: "CICS TS CECI Manager"
---

This Manager is at Release level. You can view the <a href="https://javadoc.galasa.dev/dev/galasa/cicsts/package-summary.html" target="_blank" rel="noopener noreferrer">Javadoc documentation for the Manager here</a>.


# Overview
This Manager allows Galasa tests to issue CICS/TS CECI commands.<br><br> Use the command-level interpreter (CECI) Manager to request a CECI instance in a Galasa test,  issue basic CECI commands  which can be processed interactively on a 3270 screen and  retrieve results. Examples of using this Manager can include writing data to a temporary storage  queue, linking to a CICS program or retrieving the signed on user id. <br><br> 


## Provided annotation

The following annotations are available with the CICS TS CECI Manager

??? info "CICS/TS CECI Manager"

    | Annotation: | CICS/TS CECI Manager |
    | --------------------------------------- | :------------------------------------- |
    | Name: | @CECI |
    | Description: | The <code>@CECI</code> annotation will request the CICS/TS CECI Manager to provide a CECI instance. |
    | Syntax: | @CECI<br> public ICECI ceci;<br> |
    | Notes: | Requests to the <code>ICECI</code> Manager interface requires an ITerminal object which is logged on to CICS and is at  the CECI initial screen.<br><br> If mixed case is required, the terminal should be presented with no upper case translate status. For example, the test could first issue <code>CEOT TRANIDONLY</code> to the ITerminal before invoking ICECI methods. |


## Code snippets

Use the following code snippets to help you get started with the CICS TS CECI Manager.
 
??? example "Request a CECI instance"

    The following snippet shows the code that is required to request a CECI instance in a Galasa test:

    ```
    @CEC    I
    public ICECI ceci;
    ```

    The code creates a CICS/TS CECI instance. The CECI instance will also require a 3270 terminal instance:

    ```
    @ZosImage(imageTag="A")
    public IZosImage zosImageA;

    @Zos3270Terminal(imageTag="A")
    public ITerminal ceciTerminal;
    ```
    The 3270 terminal is associated with the zOS Image allocated in the *zosImageA* field.


??? example "Issue a basic CECI command"

    The following snippet shows the code required to issue the a basic CECI command. In this case, the test will     write a message to the operator console:

    ```
    String ceciCommand = "EXEC CICS WRITE OPERATOR TEXT('About to execute Galasa Test...')";
    ICECIResponse resp = ceciTerminal.issueCommand(terminal, ceciCommand);
    if (!resp.isNormal() {
        ...
    }
    ```


??? example "Link to program with container"

    Create a CONTAINER on a CHANNEL, EXEC CICS LINK to a PROGRAM with the CHANNEL and get the returned CONTAINER data.

    Create the input CONATINER called "MY-CONTAINER-IN" on CHANNEL "MY-CHANNEL" with the data "My_Contaier_Data". The CONTAINER will default to TEXT with no code page conversion:

    ```
    ICECIResponse resp = ceci.putContainer(ceciTerminal, "MY-CHANNEL", "MY-CONTAINER-IN", "My_Contaier_Data", null, null, null);
    if (!resp.isNormal()) {
        ...
    }
    ```
    Link to PROGRAM "MYPROG" with the CHANNEL "MY-CHANNEL":

    ```
    eib = ceci.linkProgramWithChannel(ceciTerminal, "MYPROG", "MY-CHANNEL", null, null, false);
    if (!resp.isNormal()) {
    ...
    }
    ```
    Get the content of the CONTAINER "MY-CONTAINER-OUT" from CHANNEL "MY-CHANNEL" into the CECI variable "&DATAOUT" and retrieve the variable data into a String:

    ```
    eib = ceci.getContainer(ceciTerminal, "MY-CHANNEL", "MY-CONTAINER-OUT", "&DATAOUT", null, null);
    if (!resp.isNormal()) {
        ...
    }
    String dataOut = ceci.retrieveVariableText(ceciTerminal, "&DATAOUT");
    ```


??? example "Write binary data to a temporary storage queue"

    Use the following code to write binary data to TS QUEUE 

    Create a binary CECI variable:

    ```
    char[] data = {0x0C7, 0x081, 0x093, 0x081, 0x0A2, 0x081, 0x040, 0x0C4, 0x081, 0x0A3, 0x081};
    ceci.defineVariableBinary(ceciTerminal, "&BINDATA", data);
    ```
    Write the binary variable to a TS QUEUE called "MYQUEUE": 

    ```
    String command = "WRITEQ TS QUEUE('MYQUEUE') FROM(&BINDATA)";
    ICECIResponse resp = ceci.issueCommand(ceciTerminal, command);
    if (!resp.isNormal()) {
        ...
    }

    ```

    The "MYQUEUE" now contains the following data:

    ```
    Galasa Data
    ```


??? example "Confirm the signed on userid" 

    Use the following code to issue the CICS ASSIGN API and retrieve the signed on userid from the response: 


    ```
    String command = "ASSIGN";
    ICECIResponse resp = ceci.issueCommand(ceciTerminal, command);
    String userid = resp.getResponseOutputValues().get("USERID").getTextValue();

    ```

    Alternatively, issue ASSIGN and assign the userid value to a variable:

    ```
    String command = "ASSIGN USERID(&USERID)";
    ICECIResponse resp = ceci.issueCommand(ceciTerminal, command);
    String userid = ceci.retrieveVariableText("&USERID");

    ```

