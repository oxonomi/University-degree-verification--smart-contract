//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.9;

contract DegreeVerification {
    struct Degree {
        string degreeHash;
        address university;
        Status status;
        uint256 timestamp;
    }

    enum Status {
        Valid,
        Revoked,
        Modified
    }

    mapping(string => Degree) public degrees;
    address public UKAuthority;
    
    modifier onlyUKAuthority() {
        require(msg.sender == UKAuthority, "Only the UK Authority can perform this operation");
        _;
    }

    constructor() {
        UKAuthority = msg.sender;
    }

    function addUniversity(address _university) public onlyUKAuthority {
        universities[_university] = true;
    }

    function removeUniversity(address _university) public onlyUKAuthority {
        universities[_university] = false;
    }

    function addDegree(string memory _degreeHash, address _university) public {
        require(universities[msg.sender], "Only registered universities can add degrees");
        degrees[_degreeHash] = Degree(_degreeHash, _university, Status.Valid, block.timestamp);
    }

    function modifyDegree(string memory _degreeHash, Status _status) public {
        require(universities[msg.sender], "Only registered universities can modify degrees");
        degrees[_degreeHash].status = _status;
        degrees[_degreeHash].timestamp = block.timestamp;
    }

    function verifyDegree(string memory _degreeHash) public view returns (Status) {
        return degrees[_degreeHash].status;
    }

    function withdraw() public onlyUKAuthority {
        payable(UKAuthority).transfer(address(this).balance);
    }
}
