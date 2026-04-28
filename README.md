# Access-Control-with-Roles
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract RoleBasedAccess {
    address public admin;
    mapping(address => bool) public minters;

    error NotAdmin();
    error AlreadyMinter();

    event MinterAdded(address indexed minter);
    event MinterRemoved(address indexed minter);

    constructor() {
        admin = msg.sender;
    }

    modifier onlyAdmin() {
        if (msg.sender != admin) revert NotAdmin();
        _;
    }

    function addMinter(address minter) public onlyAdmin {
        if (minters[minter]) revert AlreadyMinter();
        minters[minter] = true;
        emit MinterAdded(minter);
    }

    function removeMinter(address minter) public onlyAdmin {
        minters[minter] = false;
        emit MinterRemoved(minter);
    }
}
