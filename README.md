# CorruptionReporting Smart Contract

The CorruptionReporting smart contract is designed to facilitate the reporting and verification of corruption incidents in a decentralized manner. This contract allows users to submit corruption reports, which can then be verified and rewarded by an admin. The contract uses Solidity's require(), assert(), and revert() statements to ensure the integrity and correctness of its operations.


## Description
The CorruptionReporting smart contract provides a decentralized platform for reporting, verifying, and rewarding corruption reports. It ensures the integrity and reliability of the reporting process through the use of require(), assert(), and revert() statements. The contract includes functionalities for report submission, verification by an admin, and reward issuance for verified reports.
## Getting Started
## Executing Program
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at https://remix.ethereum.org/.

Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a .sol extension (e.g., avax.sol). Copy and paste the following code into the file:




bash

    //pragma solidity ^0.8.0;

    contract CorruptionReporting {
    struct IncidentReport {
        address reporterAddress;
        string reportDescription;
        bool isVerified;
        bool isRewarded;
    }

    address public contractAdmin;
    uint public totalReports;
    mapping(uint => IncidentReport) public incidentReports;

    event ReportSubmitted(uint indexed reportId, address indexed reporterAddress);
    event ReportVerified(uint indexed reportId);
    event RewardIssued(uint indexed reportId);

    modifier onlyContractAdmin() {
        require(msg.sender == contractAdmin, "Caller is not the contract admin");
        _;
    }

    constructor() {
        contractAdmin = msg.sender;
    }

    function submitIncidentReport(string memory _reportDescription) public {
        require(bytes(_reportDescription).length > 0, "Report description is required");

        incidentReports[++totalReports] = IncidentReport(msg.sender, _reportDescription, false, false);
        emit ReportSubmitted(totalReports, msg.sender);
    }

    function verifyIncidentReport(uint _reportId) public onlyContractAdmin {
        require(_reportId > 0 && _reportId <= totalReports, "Invalid report ID");

        IncidentReport storage report = incidentReports[_reportId];
        require(!report.isVerified, "Report already verified");

        report.isVerified = true;
        emit ReportVerified(_reportId);
    }

    function issueReward(uint _reportId) public onlyContractAdmin {
        require(_reportId > 0 && _reportId <= totalReports, "Invalid report ID");

        IncidentReport storage report = incidentReports[_reportId];
        require(report.isVerified && !report.isRewarded, "Invalid reward state");

        report.isRewarded = true;
        (bool success, ) = report.reporterAddress.call{value: 1 ether}("");
        require(success, "Reward transfer failed");

        emit RewardIssued(_reportId);
    }

    function checkInvariant() public view {
        assert(totalReports >= 0);
    }

    function forceRevert(string memory _reason) public pure {
        revert(_reason);
    }

    receive() external payable {}
    }


To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar. Make sure the "Compiler" option is set to "0.8.0" (or another compatible version), and then click on the "Compile Assesment.sol" button.

Once the code is compiled, you can deploy the contract by clicking on the "Deploy & Run Transactions" tab in the left-hand sidebar. Ensure "CorruptionReporting" is selected in the contract dropdown.
Click on the "Deploy" button.

In the "Deployed Contracts" section, expand the CorruptionReporting contract.Under the "submitReport" function, enter the report description.Click the "transact" button next to the submitReport function.

Under the "verifyReport" function, enter the report ID.Click the "transact" button next to the verifyReport function.Confirm the transaction in MetaMask.







## Authors

- Shiwani
shivuagarwal23@gmail.com



## License

This project is licensed under [MIT](https://choosealicense.com/licenses/mit/) License - see the LICENSE.md file for details
