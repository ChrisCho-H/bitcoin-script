# bitcoin-script

bitcoin script(opcode) guide in detail ✨

```cpp
/** Script opcodes */
enum opcodetype
{
/**
 * Opcodes that take a true/false value will evaluate the following as false:
 *     an empty vector
 *     a vector (of any length) of all zero bytes
 *     a single byte of "\x80" ('negative zero')
 *     a vector (of any length) of all zero bytes except the last byte is "\x80"
 *
 * Any other value will evaluate to true.
 *
 * Unless specified otherwise, all opcodes below only have an effect when they occur in executed branches.
 */
    /** push value */
    /**
     * both opcodes below push "" onto the stack (which is an empty array of bytes)
     */
    OP_0 = 0x00,
    OP_FALSE = OP_0,
    /**
     * read the next byte as N and push the next N bytes as an array onto the stack
     */
    OP_PUSHDATA1 = 0x4c,
    /**
     * read the next 2 bytes as N and push the next N bytes as an array onto the stack
     */
    OP_PUSHDATA2 = 0x4d,
    /**
     * read the next 4 bytes as N and push the next N bytes as an array onto the stack
     */
    OP_PUSHDATA4 = 0x4e,
    /**
     * push "\x81" onto the stack (which is interpreted as -1 by numerical opcodes)
     */
    OP_1NEGATE = 0x4f,
    /**
     * mark transaction invalid
     * turned into OP_SUCCESS80 in tapscript
     */
    OP_RESERVED = 0x50,
    /**
     * both opcodes below push "\x01" onto the stack (which is interpreted as 1 by numerical opcodes)
     */
    OP_1 = 0x51,
    OP_TRUE = OP_1,
    /**
     * push "\x02" onto the stack (which is interpreted as 2 by numerical opcodes)
     */
    OP_2 = 0x52,
    /**
     * push "\x03" onto the stack (which is interpreted as 3 by numerical opcodes)
     */
    OP_3 = 0x53,
    /**
     * push "\x04" onto the stack (which is interpreted as 4 by numerical opcodes)
     */
    OP_4 = 0x54,
    /**
     * push "\x05" onto the stack (which is interpreted as 5 by numerical opcodes)
     */
    OP_5 = 0x55,
    /**
     * push "\x06" onto the stack (which is interpreted as 6 by numerical opcodes)
     */
    OP_6 = 0x56,
    /**
     * push "\x07" onto the stack (which is interpreted as 7 by numerical opcodes)
     */
    OP_7 = 0x57,
    /**
     * push "\x08" onto the stack (which is interpreted as 8 by numerical opcodes)
     */
    OP_8 = 0x58,
    /**
     * push "\x09" onto the stack (which is interpreted as 9 by numerical opcodes)
     */
    OP_9 = 0x59,
    /**
     * push "\x0A" onto the stack (which is interpreted as 10 by numerical opcodes)
     */
    OP_10 = 0x5a,
    /**
     * push "\x0B" onto the stack (which is interpreted as 11 by numerical opcodes)
     */
    OP_11 = 0x5b,
    /**
     * push "\x0C" onto the stack (which is interpreted as 12 by numerical opcodes)
     */
    OP_12 = 0x5c,
    /**
     * push "\x0D" onto the stack (which is interpreted as 13 by numerical opcodes)
     */
    OP_13 = 0x5d,
    /**
     * push "\x0E" onto the stack (which is interpreted as 14 by numerical opcodes)
     */
    OP_14 = 0x5e,
    /**
     * push "\x0F" onto the stack (which is interpreted as 15 by numerical opcodes)
     */
    OP_15 = 0x5f,
    /**
     * push "\x10" onto the stack (which is interpreted as 16 by numerical opcodes)
     */
    OP_16 = 0x60,

    /** control */
    /**
     * do nothing
     */
    OP_NOP = 0x61,
    /**
     * opcode below is disabled
     * mark transaction invalid
     * turned into OP_SUCCESS98 in tapscript
     */
    OP_VER = 0x62,
    /**
     * if top stack value is true (exactly "\x01" for tapscript), execute the statement
     */
    OP_IF = 0x63,
    /**
     * if top stack value is false ("" for tapscript), execute the statement
     */
    OP_NOTIF = 0x64,
    /**
     * both opcodes below are disabled
     * mark transaction invalid even when occurring in an unexecuted branch
     */
    OP_VERIF = 0x65,
    OP_VERNOTIF = 0x66,
    /**
     * if the preceding OP_IF, OP_NOTIF or OP_ELSE not executed, execute the statement
     */
    OP_ELSE = 0x67,
    /**
     * end if/else block (must include, otherwise tx becomes invalid)
     */
    OP_ENDIF = 0x68,
    /**
     * mark transaction invalid if top stack value is false
     */
    OP_VERIFY = 0x69,
    /**
     * mark transaction invalid
     */
    OP_RETURN = 0x6a,

    /** stack ops */
    /**
     * pop an item from the main stack onto the alt stack
     */
    OP_TOALTSTACK = 0x6b,
    /**
     * pop an item from the alt stack onto the main stack
     */
    OP_FROMALTSTACK = 0x6c,
    /**
     * remove the two top stack items
     * [ ... x0 x1 ] -> [ ... ]
     */
    OP_2DROP = 0x6d,
    /**
     * duplicate top and second from top stack items
     * [ ... x0 x1 ] -> [ ... x0 x1 x0 x1 ]
     */
    OP_2DUP = 0x6e,
    /**
     * duplicate top, second from top and third from top stack items
     * [ ... x0 x1 x2 ] -> [ ... x0 x1 x2 x0 x1 x2 ]
     */
    OP_3DUP = 0x6f,
    /**
     * copy third and fourth from top stack items to the top
     * [ ... x0 x1 x2 x3 ] -> [ ... x0 x1 x2 x3 x0 x1 ]
     */
    OP_2OVER = 0x70,
    /**
     * move fifth and sixth from top stack items to the top
     * [ ... x0 x1 x2 x3 x4 x5 ] -> [ ... x2 x3 x4 x5 x0 x1 ]
     */
    OP_2ROT = 0x71,
    /**
     * swap: top and second from top <-> third and fourth from top items of stack
     * [ ... x0 x1 x2 x3 ] -> [ ... x2 x3 x0 x1 ]
     */
    OP_2SWAP = 0x72,
    /**
     * if top stack value is true, OP_DUP
     */
    OP_IFDUP = 0x73,
    /**
     * push the current number of stack items onto the stack
     * [ x0 x1 ... xN ] -> [ x0 x1 ... xN N+1 ]
     */
    OP_DEPTH = 0x74,
    /**
     * remove top stack item
     * [ ... x0 ] -> [ ... ]
     */
    OP_DROP = 0x75,
    /**
     * duplicate top stack item
     * [ ... x0 ] -> [ ... x0 x0 ]
     */
    OP_DUP = 0x76,
    /**
     * remove second from top stack item
     * [ ... x0 x1 ] -> [ ... x1 ]
     */
    OP_NIP = 0x77,
    /**
     * copy second from top stack item to the top
     * [ ... x0 x1 ] -> [ ... x0 x1 x0 ]
     */
    OP_OVER = 0x78,
    /**
     * copy item N back in stack to the top
     * [ ... x0 x1 ... xN N+1 ] -> [ ... x0 x1 ... xN x0 ]
     */
    OP_PICK = 0x79,
    /**
     * move item N back in stack to the top
     * [ ... x0 x1 ... xN N+1 ] -> [ ... x1 ... xN x0 ]
     */
    OP_ROLL = 0x7a,
    /**
     * move third from top stack item to the top
     * [ ... x0 x1 x2 ] -> [ ... x1 x2 x0 ]
     */
    OP_ROT = 0x7b,
    /**
     * swap top two items of stack
     * [ ... x0 x1 ] -> [ ... x1 x0]
     */
    OP_SWAP = 0x7c,
    /**
     * copy top stack item and insert before second from top item
     * [ ... x0 x1 ] -> [ ... x1 x0 x1 ]
     */
    OP_TUCK = 0x7d,

    /** splice ops */
    /**
     * opcodes below until OP_SIZE are disabled
     * mark transaction invalid even when occurring in an unexecuted branch
     * turned into OP_SUCCESS126-129 in tapscript
     */
    OP_CAT = 0x7e,
    OP_SUBSTR = 0x7f,
    OP_LEFT = 0x80,
    OP_RIGHT = 0x81,
    /**
     * push the length of top stack item (not pop the top element whose size is inspected)
     */
    OP_SIZE = 0x82,

    /** bit logic */
    /**
     * opcodes below until OP_EQUAL are disabled
     * mark transaction invalid even when occurring in an unexecuted branch
     * turned into OP_SUCCESS131-134 in tapscript
     */
    OP_INVERT = 0x83,
    OP_AND = 0x84,
    OP_OR = 0x85,
    OP_XOR = 0x86,
    /**
     * pop two top stack items and push "\x01" if they are equal, otherwise ""
     */
    OP_EQUAL = 0x87,
    /**
     * execute OP_EQUAL, then OP_VERIFY afterward
     */
    OP_EQUALVERIFY = 0x88,
    /**
     * both opcodes below mark transaction invalid
     * turned into OP_SUCCESS137-138 in tapscript
     */
    OP_RESERVED1 = 0x89,
    OP_RESERVED2 = 0x8a,

    /** numeric */
    /**
     * add 1 to the top stack item
     */
    OP_1ADD = 0x8b,
    /**
     * subtract 1 from the top stack item
     */
    OP_1SUB = 0x8c,
    /**
     * both opcodes below are disabled
     * mark transaction invalid even when occurring in an unexecuted branch
     * turned into OP_SUCCESS141-142 in tapscript
     */
    OP_2MUL = 0x8d,
    OP_2DIV = 0x8e,
    /**
     * multiply the top stack item by -1
     */
    OP_NEGATE = 0x8f,
    /**
     * replace top stack item by its absolute value
     */
    OP_ABS = 0x90,
    /**
     * replace top stack item by "\x01" if its value is 0, otherwise ""
     */
    OP_NOT = 0x91,
    /**
     * replace top stack item by "" if its value is 0, otherwise by "\x01"
     */
    OP_0NOTEQUAL = 0x92,

    /**
     * pop two top stack items and push their sum
     */
    OP_ADD = 0x93,
    /**
     * pop two top stack items and push the second minus the top
     */
    OP_SUB = 0x94,
    /**
     * opcodes below until OP_BOOLAND are disabled
     * mark transaction invalid even when occurring in an unexecuted branch
     * turned into OP_SUCCESS149-153 in tapscript
     */
    OP_MUL = 0x95,
    OP_DIV = 0x96,
    OP_MOD = 0x97,
    OP_LSHIFT = 0x98,
    OP_RSHIFT = 0x99,

    /**
     * pop two top stack items and push "\x01" if they are both true, "" otherwise
     */
    OP_BOOLAND = 0x9a,
    /**
     * pop two top stack items and push "\x01" if top or second from top stack item is not 0, otherwise ""
     */
    OP_BOOLOR = 0x9b,
    /**
     * pop two top stack items and push "\x01" if inputs are equal, otherwise ""
     */
    OP_NUMEQUAL = 0x9c,
    /**
     * execute OP_NUMEQUAL, then OP_VERIFY afterward
     */
    OP_NUMEQUALVERIFY = 0x9d,
    /**
     * pop two top stack items and push "\x01" if inputs are not equal, otherwise ""
     */
    OP_NUMNOTEQUAL = 0x9e,
    /**
     * pop two top stack items and push "\x01" if second from top < top, otherwise ""
     */
    OP_LESSTHAN = 0x9f,
    /**
     * pop two top stack items and push "\x01" if second from top > top, otherwise ""
     */
    OP_GREATERTHAN = 0xa0,
    /**
     * pop two top stack items and push "\x01" if second from top <= top, otherwise ""
     */
    OP_LESSTHANOREQUAL = 0xa1,
    /**
     * pop two top stack items and push "\x01" if second from top >= top, otherwise ""
     */
    OP_GREATERTHANOREQUAL = 0xa2,
    /**
     * pop two top stack items and push the smaller
     */
    OP_MIN = 0xa3,
    /**
     * pop two top stack items and push the bigger
     */
    OP_MAX = 0xa4,

    /**
     * pop three top stack items and push "\x01" if third from top > top >= second from top, otherwise ""
     */
    OP_WITHIN = 0xa5,

    /** crypto */
    /**
     * replace top stack item by its RIPEMD-160 hash
     */
    OP_RIPEMD160 = 0xa6,
    /**
     * replace top stack item by its SHA-1 hash
     */
    OP_SHA1 = 0xa7,
    /**
     * replace top stack item by its SHA-256 hash
     */
    OP_SHA256 = 0xa8,
    /**
     * replace top stack item by hashing SHA-256 then RIPEMD-160
     */
    OP_HASH160 = 0xa9,
    /**
     * replace top stack item by hashing SHA-256 twice
     */
    OP_HASH256 = 0xaa,
    /**
     * all of the signature checking words will only match signatures to the data
     * after the most recently-executed OP_CODESEPARATOR
     */
    OP_CODESEPARATOR = 0xab,
    /**
     * push "\x01" if signature is valid for tx hash and public key, otherwise ""
     */
    OP_CHECKSIG = 0xac,
    /**
     * execute OP_CHECKSIG, then OP_VERIFY afterward
     */
    OP_CHECKSIGVERIFY = 0xad,
    /**
     * OP_CHECKSIG for multiple signatures
     */
    OP_CHECKMULTISIG = 0xae,
    /**
     * execute OP_CHECKMULTISIG, then OP_VERIFY afterward
     */
    OP_CHECKMULTISIGVERIFY = 0xaf,

    /** expansion */
    /**
     * do nothing
     */
    OP_NOP1 = 0xb0,
    /**
     * both below are absolute lock time (check details in BIP65)
     */
    OP_CHECKLOCKTIMEVERIFY = 0xb1,
    OP_NOP2 = OP_CHECKLOCKTIMEVERIFY,
    /**
     * both below are relative lock time (check details in BIP68 and BIP112)
     */
    OP_CHECKSEQUENCEVERIFY = 0xb2,
    OP_NOP3 = OP_CHECKSEQUENCEVERIFY,
    /**
     * opcodes below do nothing
     */
    OP_NOP4 = 0xb3,
    OP_NOP5 = 0xb4,
    OP_NOP6 = 0xb5,
    OP_NOP7 = 0xb6,
    OP_NOP8 = 0xb7,
    OP_NOP9 = 0xb8,
    OP_NOP10 = 0xb9,

    /**
     * Opcode added by BIP 342 (Tapscript)
     * pop the public key, N and a signature, push N if signature is empty,
     * fail if it's invalid, otherwise push N + 1 (see BIP 342)
     */
    OP_CHECKSIGADD = 0xba,

    OP_INVALIDOPCODE = 0xff,
};
```
