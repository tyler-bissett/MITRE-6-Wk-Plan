# Security Requirements Tracking

This document tracks the main security requirements for the MITRE eCTF 2025 project. All implementation work should be done in the [2025-ectf-capstone repository](https://github.com/uscga-ectf/2025-ectf-capstone).

## Critical Security Requirements

### 1. Cryptographic Protection

#### 1.1 AES-GCM Encryption (Issue #10, #15)
**Requirement**: Replace basic AES with hardware-accelerated AES-GCM mode using MAX78000 MXC API

**Status**: Open  
**Priority**: High  
**Issue Reference**: #10, #15

**Verification Criteria**:
- [ ] simple_crypto.c modified to use MXC AES API in GCM mode
- [ ] Hardware acceleration properly configured and tested
- [ ] Legacy AES mode removed (if applicable)
- [ ] Performance benchmarks show improvement over software implementation

**Security Impact**: Provides both confidentiality and authenticity through Authenticated Encryption with Associated Data (AEAD)

---

#### 1.2 AEAD Frame Integrity Verification (Issue #11)
**Requirement**: Add AEAD verification for frame integrity to prevent tampering

**Status**: Open  
**Priority**: High  
**Issue Reference**: #11

**Verification Criteria**:
- [ ] AEAD tags generated for all frames
- [ ] Tag verification performed before frame processing
- [ ] Tampered frames properly rejected
- [ ] Error handling in place for failed verifications

**Security Impact**: Ensures frame data integrity and authenticity, prevents man-in-the-middle attacks

---

### 2. Message Authentication

#### 2.1 UART Message MAC (Issue #12)
**Requirement**: Wrap UART messages with Message Authentication Code (MAC) using either HMAC or GCM-SIV-style authentication tags

**Status**: Open  
**Priority**: High  
**Issue Reference**: #12

**Implementation Notes**: Choose either HMAC-SHA256 or GCM-SIV authentication tags based on hardware support and performance requirements. GCM-SIV provides nonce-misuse resistance if using AES-GCM hardware.

**Verification Criteria**:
- [ ] Selected MAC algorithm (HMAC-SHA256 or GCM-SIV) implemented for all UART packets
- [ ] Authentication tags verified before parsing messages
- [ ] Invalid or missing tags properly rejected
- [ ] MAC algorithm properly implemented and tested
- [ ] Tag length and key management properly handled

**Security Impact**: Prevents unauthorized message injection and tampering on UART communication channel

---

#### 2.2 Subscription HMAC Signatures (Issue #13, #14)
**Requirement**: Sign subscriptions with deployment key and verify in decoder

**Status**: Open  
**Priority**: High  
**Issue Reference**: #13, #14

**Verification Criteria**:
- [ ] gen_subscription.py appends HMAC-SHA256 signatures
- [ ] Signatures include version field
- [ ] verify_subscription_hmac implemented in decoder
- [ ] Unsigned or altered subscriptions rejected
- [ ] Subscriptions only stored after successful HMAC verification

**Security Impact**: Ensures only authorized subscriptions are accepted, prevents subscription forgery

---

### 3. Replay Attack Protection

#### 3.1 Timestamp Persistence (Issue #9)
**Requirement**: Store last-seen timestamp in non-volatile memory and reject non-monotonic frames

**Status**: Open  
**Priority**: High  
**Issue Reference**: #9

**Verification Criteria**:
- [ ] Last-seen timestamp stored in non-volatile memory
- [ ] Timestamp checking implemented for all frames
- [ ] Non-monotonic frames rejected
- [ ] Persistence survives device reboot
- [ ] Clock synchronization properly handled

**Security Impact**: Prevents replay attacks by ensuring frames cannot be re-sent after being captured

---

### 4. Access Control

#### 4.1 Subscription-Based Frame Authorization (Issue #8, #26)
**Requirement**: Gate decoding by device_id, channel, and time window from subscription

**Status**: Open  
**Priority**: High  
**Issue Reference**: #8, #26

**Verification Criteria**:
- [ ] Frame decoding checks device_id against subscription
- [ ] Channel authorization verified from subscription
- [ ] Time window enforcement implemented
- [ ] Frames outside authorization rejected
- [ ] Subscription checks pass all test cases

**Security Impact**: Ensures only authorized devices can decode specific frames, enforces access control policy

---

### 5. Cryptographic Library Integration

#### 5.1 WolfCrypt Integration (Issue #28)
**Requirement**: Get wolfcrypt working in project for cryptographic operations

**Status**: Open  
**Priority**: Medium  
**Issue Reference**: #28

**Verification Criteria**:
- [ ] WolfCrypt library properly integrated into build system
- [ ] All necessary cryptographic primitives available
- [ ] Library passes initialization and self-tests
- [ ] No conflicts with existing crypto implementations
- [ ] Documentation updated with usage guidelines

**Security Impact**: Provides well-tested, industry-standard cryptographic primitives

---

## Security Testing Requirements

### Build and Compilation
- [ ] Make compilation works correctly (Issue #23)
- [ ] All security features compile without warnings
- [ ] No debug code or test keys in production builds

### Testing and Debugging
- [ ] Debugging methods established (Issue #24)
- [ ] Security features tested in isolation
- [ ] Integration tests verify end-to-end security
- [ ] Negative tests verify proper rejection of invalid/malicious inputs

### Regression Testing
- [ ] Regression testing documentation complete (Issue #16)
- [ ] Security regression tests defined
- [ ] Automated testing where possible

---

## Documentation Requirements

### Weekly Documentation (Issues #17-22)
- [ ] Week 1 work documented in release.md
- [ ] Week 2 work documented in release.md
- [ ] Week 3 work documented in release.md
- [ ] Week 4 work documented
- [ ] Week 5 work documented
- [ ] Week 6 work documented

### Requirements Documentation
- [x] Requirements matrix completed (Issue #27)
- [x] Requirements doc resubmitted (Issue #25)

---

## Summary

**Total Security Requirements**: 8 critical requirements  
**Completed**: 0  
**In Progress**: 0  
**Not Started**: 8  

**Next Steps**:
1. Prioritize AES-GCM implementation (Issues #10, #15)
2. Implement UART MAC and subscription HMAC (Issues #12, #13, #14)
3. Add AEAD verification for frames (Issue #11)
4. Implement replay attack protection (Issue #9)
5. Enforce subscription-based authorization (Issue #8, #26)
6. Integrate WolfCrypt library (Issue #28)

**Critical Path**: Items 1-4 should be completed before final security review.

---

## Notes

- All actual implementation work must be done in the [2025-ectf-capstone repository](https://github.com/uscga-ectf/2025-ectf-capstone)
- This document should be updated as requirements are completed
- Each requirement should be independently verified before marking complete
- Security features should be tested both in isolation and as an integrated system
- Consider having security experts review the implementation before deployment

---

*Last Updated*: November 1, 2025  
*Document Version*: 1.0
