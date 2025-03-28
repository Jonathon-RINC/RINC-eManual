---
layout: archive
lang: en
ref: cpp_dxl_monitor
read_time: true
share: true
author_profile: false
permalink: /docs/en/software/dynamixel/dynamixel_sdk/sample_code/cpp_dxl_monitor/
sidebar:
  title: DYNAMIXEL SDK
  nav: "dynamixel_sdk"
---

<style>body {counter-reset: h1 5 !important;}</style>
<div style="counter-reset: h2 7"></div>

<!--[dummy Header 1]>
  <h1 id="sample-code"><a href="#sample-code">Sample Code</a></h1>
<![end dummy Header 1]-->

## [CPP DXL Monitor](#cpp-dxl-monitor)

- Description

  In this example, all fuctions except syncread, syncwrite, bulkread, bulkwrite appears on the command prompt or terminal. User may type commands to use the functions. For example:

  Type below (use Dynamixel PRO of ID = 1)

  `[CMD] wrb2 1 7 2                        //wrb2 [ID] [ADDRESS] [Value]`

  and type enter. Then, the Dynamixel PRO’s ID would be changed to ID = 2. To see another commands, type “help” or ‘h’, or ‘?’ to show the help.

  Each command is classified by numbering on the end of the command string. For instance, command “wrb2” will “write byte using Protocol 2.0”. On the other hand, command that doesn’t have number at the end, such as “scan”, will “scan and show list of connected DYNAMIXEL and the model number of the Dynamixel, using both protocols”

- Available Dynamixel

  All series using protocol 1.0 and 2.0


### Sample code

``` cpp
/*
 * dxl_monitor.cpp
 *
 *  Created on: 2016. 2. 21.
 *      Author: leon
 */

//
// *********     DXL Monitor Example      *********
//
//
// Available Dynamixel model on this example : All models using Protocol 1.0 and 2.0
// This example is tested with a Dynamixel MX-28, a Dynamixel PRO 54-200 and an USB2DYNAMIXEL
// To use another Dynamixel model, such as X series, see their details in E-Manual(support.robotis.com) and edit below "#define"d variables yourself.
// Be sure that properties of Dynamixel MX and PRO are already set as %% MX - ID : 1 / Baudnum : 1 (Baudrate : 1000000 [1M]) , PRO - ID : 1 / Baudnum : 3 (Baudrate : 1000000 [1M])
//

#ifdef __linux__
#include <unistd.h>
#include <fcntl.h>
#include <getopt.h>
#include <termios.h>
#elif defined(_WIN32) || defined(_WIN64)
#include <conio.h>
#endif

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <vector>

#include "dynamixel_sdk.h"                                  // Uses DYNAMIXEL SDK library

// Protocol version
#define PROTOCOL_VERSION1               1.0                 // See which protocol version is used in the Dynamixel
#define PROTOCOL_VERSION2               2.0

// Default setting
#define DEVICENAME                      "/dev/ttyUSB0"      // Check which port is being used on your controller
                                                            // ex) Windows: "COM1"   Linux: "/dev/ttyUSB0"

int getch()
{
#ifdef __linux__
  struct termios oldt, newt;
  int ch;
  tcgetattr(STDIN_FILENO, &oldt);
  newt = oldt;
  newt.c_lflag &= ~(ICANON | ECHO);
  tcsetattr(STDIN_FILENO, TCSANOW, &newt);
  ch = getchar();
  tcsetattr(STDIN_FILENO, TCSANOW, &oldt);
  return ch;
#elif defined(_WIN32) || defined(_WIN64)
  return _getch();
#endif
}

int kbhit(void)
{
#ifdef __linux__
  struct termios oldt, newt;
  int ch;
  int oldf;

  tcgetattr(STDIN_FILENO, &oldt);
  newt = oldt;
  newt.c_lflag &= ~(ICANON | ECHO);
  tcsetattr(STDIN_FILENO, TCSANOW, &newt);
  oldf = fcntl(STDIN_FILENO, F_GETFL, 0);
  fcntl(STDIN_FILENO, F_SETFL, oldf | O_NONBLOCK);

  ch = getchar();

  tcsetattr(STDIN_FILENO, TCSANOW, &oldt);
  fcntl(STDIN_FILENO, F_SETFL, oldf);

  if (ch != EOF)
  {
    ungetc(ch, stdin);
    return 1;
  }

  return 0;
#elif defined(_WIN32) || defined(_WIN64)
  return _kbhit();
#endif
}

void Usage(char *progname)
{
  printf("-----------------------------------------------------------------------\n");
  printf("Usage: %s\n");
  printf(" [-h | --help]........: display this help\n");
  printf("[-d | --device]......: port to open\n", progname);
  printf("-----------------------------------------------------------------------\n");
}

void Help()
{
  printf("\n");
  printf("                    .----------------------------.\n");
  printf("                    |  DXL Monitor Command List  |\n");
  printf("                    '----------------------------'\n");
  printf(" =========================== Common Commands ===========================\n");
  printf(" \n");
  printf(" help|h|?                    :Displays help information\n");
  printf(" baud [BAUD_RATE]            :Changes baudrate to [BAUD_RATE] \n");
  printf("                               ex) baud 2400 (2400 bps) \n");
  printf("                               ex) baud 1000000 (1 Mbps)  \n");
  printf(" exit                        :Exit this program\n");
  printf(" scan                        :Outputs the current status of all DYNAMIXEL\n");
  printf(" ping [ID] [ID] ...          :Outputs the current status of [ID]s \n");
  printf(" bp                          :Broadcast ping (Dynamixel Protocol 2.0 only)\n");
  printf(" \n");
  printf(" ==================== Commands for Dynamixel Protocol 1.0 ====================\n");
  printf(" \n");
  printf(" wrb1|w1 [ID] [ADDR] [VALUE] :Write byte [VALUE] to [ADDR] of [ID]\n");
  printf(" wrw1 [ID] [ADDR] [VALUE]    :Write word [VALUE] to [ADDR] of [ID]\n");
  printf(" rdb1 [ID] [ADDR]            :Read byte value from [ADDR] of [ID]\n");
  printf(" rdw1 [ID] [ADDR]            :Read word value from [ADDR] of [ID]\n");
  printf(" r1 [ID] [ADDR] [LENGTH]     :Dumps the control table of [ID]\n");
  printf("                               ([LENGTH] bytes from [ADDR])\n");
  printf(" reset1|rst1 [ID]            :Factory reset the Dynamixel of [ID]\n");
  printf(" \n");
  printf(" ==================== Commands for Dynamixel Protocol 2.0 ====================\n");
  printf(" \n");
  printf(" wrb2|w2 [ID] [ADDR] [VALUE] :Write byte [VALUE] to [ADDR] of [ID]\n");
  printf(" wrw2 [ID] [ADDR] [VALUE]    :Write word [VALUE] to [ADDR] of [ID]\n");
  printf(" wrd2 [ID] [ADDR] [VALUE]    :Write dword [VALUE] to [ADDR] of [ID]\n");
  printf(" rdb2 [ID] [ADDR]            :Read byte value from [ADDR] of [ID]\n");
  printf(" rdw2 [ID] [ADDR]            :Read word value from [ADDR] of [ID]\n");
  printf(" rdd2 [ID] [ADDR]            :Read dword value from [ADDR] of [ID]\n");
  printf(" r2 [ID] [ADDR] [LENGTH]     :Dumps the control table of [ID]\n");
  printf("                               ([LENGTH] bytes from [ADDR])\n");
  printf(" reboot2|rbt2 [ID]           :reboot the Dynamixel of [ID]\n");
  printf(" reset2|rst2 [ID] [OPTION]   :Factory reset the Dynamixel of [ID]\n");
  printf("                               OPTION: 255(All), 1(Except ID), 2(Except ID&Baud)\n");

  printf("\n");
}

void Scan(dynamixel::PortHandler *portHandler, dynamixel::PacketHandler *packetHandler1, dynamixel::PacketHandler *packetHandler2)
{
  uint8_t dxl_error;
  uint16_t dxl_model_num;

  fprintf(stderr, "\n");
  fprintf(stderr, "Scan Dynamixel Using Protocol 1.0\n");
  for (int id = 1; id < 253; id++)
  {
    if (packetHandler1-> ping(portHandler, id, &dxl_model_num, &dxl_error)== COMM_SUCCESS)
    {
      fprintf(stderr, "\n                                          ... SUCCESS \r");
      fprintf(stderr, " [ID:%.3d] Model No : %.5d \n", id, dxl_model_num);
    }
    else
        fprintf(stderr, ".");

    if (kbhit())
    {
        char c = getch();
        if (c == 0x1b)
        break;
    }
  }
  fprintf(stderr, "\n\n");

  fprintf(stderr, "Scan Dynamixel Using Protocol 2.0\n");
  for (int id = 1; id < 253; id++)
  {
    if (packetHandler2-> ping(portHandler, id, &dxl_model_num, &dxl_error)== COMM_SUCCESS)
    {
      fprintf(stderr, "\n                                          ... SUCCESS \r");
      fprintf(stderr, " [ID:%.3d] Model No : %.5d \n", id, dxl_model_num);
    }
    else
    {
      fprintf(stderr, ".");
    }

    if (kbhit())
    {
      char c = getch();
      if (c == 0x1b) break;
    }
  }
  fprintf(stderr, "\n\n");
}

void Write(dynamixel::PortHandler *portHandler, dynamixel::PacketHandler *packetHandler, uint8_t id, uint16_t addr, uint16_t length, uint32_t value)
{
  uint8_t dxl_error = 0;
  int dxl_comm_result = COMM_TX_FAIL;

  if (length == 1)
  {
    dxl_comm_result = packetHandler->write1ByteTxRx(portHandler, id, addr, (uint8_t)value, &dxl_error);
  }
  else if (length == 2)
  {
    dxl_comm_result = packetHandler->write2ByteTxRx(portHandler, id, addr, (uint16_t)value, &dxl_error);
  }
  else if (length == 4)
  {
    dxl_comm_result = packetHandler->write4ByteTxRx(portHandler, id, addr, (uint32_t)value, &dxl_error);
  }

  if (dxl_comm_result == COMM_SUCCESS)
  {
    if (dxl_error != 0) packetHandler->printRxPacketError(dxl_error);
    fprintf(stderr, "\n Success to write\n\n");
  }
  else
  {
    packetHandler->printTxRxResult(dxl_comm_result);
    fprintf(stderr, "\n Fail to write! \n\n");
  }
}

void Read(dynamixel::PortHandler *portHandler, dynamixel::PacketHandler *packetHandler, uint8_t id, uint16_t addr, uint16_t length)
{
  uint8_t dxl_error = 0;
  int     dxl_comm_result = COMM_TX_FAIL;

  int8_t  value8    = 0;
  int16_t value16   = 0;
  int32_t value32   = 0;


  if (length == 1)
  {
    dxl_comm_result = packetHandler->read1ByteTxRx(portHandler, id, addr, (uint8_t*)&value8, &dxl_error);
  }
  else if (length == 2)
  {
    dxl_comm_result = packetHandler->read2ByteTxRx(portHandler, id, addr, (uint16_t*)&value16, &dxl_error);
  }
  else if (length == 4)
  {
    dxl_comm_result = packetHandler->read4ByteTxRx(portHandler, id, addr, (uint32_t*)&value32, &dxl_error);
  }

  if (dxl_comm_result == COMM_SUCCESS)
  {
    if (dxl_error != 0) packetHandler->printRxPacketError(dxl_error);

    if (length == 1)
    {
      fprintf(stderr, "\n READ VALUE : (UNSIGNED) %u , (SIGNED) %d \n\n", (uint8_t)value8, value8);
    }
    else if (length == 2)
    {
      fprintf(stderr, "\n READ VALUE : (UNSIGNED) %u , (SIGNED) %d \n\n", (uint16_t)value16, value16);
    }
    else if (length == 4)
    {
      fprintf(stderr, "\n READ VALUE : (UNSIGNED) %u , (SIGNED) %d \n\n", (uint32_t)value32, value32);
    }
  }
  else
  {
    packetHandler->printTxRxResult(dxl_comm_result);
    fprintf(stderr, "\n Fail to read! \n\n");
  }
}

void Dump(dynamixel::PortHandler *portHandler, dynamixel::PacketHandler *packetHandler, uint8_t id, uint16_t addr, uint16_t len)
{
  uint8_t  dxl_error       = 0;
  int      dxl_comm_result = COMM_TX_FAIL;
  uint8_t *data            = (uint8_t*)calloc(len, sizeof(uint8_t));

  dxl_comm_result = packetHandler->readTxRx(portHandler, id, addr, len, data, &dxl_error);
  if (dxl_comm_result == COMM_SUCCESS)
  {
    if (dxl_error != 0)
      packetHandler->printRxPacketError(dxl_error);

    if (id != BROADCAST_ID)
    {
      fprintf(stderr, "\n");
      for (int i = addr; i < addr+len; i++)
      fprintf(stderr, "ADDR %.3d [0x%.4X] :     %.3d [0x%.2X] \n", i, i, data[i-addr], data[i-addr]);
      fprintf(stderr, "\n");
    }
  }
  else
  {
    packetHandler->printTxRxResult(dxl_comm_result);
    fprintf(stderr, "\n Fail to read! \n\n");
  }

  free(data);
}

int main(int argc, char *argv[])
{
  // Initialize Packethandler1 instance
  dynamixel::PacketHandler *packetHandler1 = dynamixel::PacketHandler::getPacketHandler(PROTOCOL_VERSION1);

  // Initialize Packethandler2 instance
  dynamixel::PacketHandler *packetHandler2 = dynamixel::PacketHandler::getPacketHandler(PROTOCOL_VERSION2);

  fprintf(stderr, "\n***********************************************************************\n");
  fprintf(stderr,   "*                            DXL Monitor                              *\n");
  fprintf(stderr,   "***********************************************************************\n\n");

  char *dev_name = (char*)DEVICENAME;

#ifdef __linux__
  // parameter parsing
  while(1)
  {
    int option_index = 0, c = 0;
    static struct option long_options[] = {
        {"h", no_argument, 0, 0},
        {"help", no_argument, 0, 0},
        {"d", required_argument, 0, 0},
        {"device", required_argument, 0, 0},
        {0, 0, 0, 0}
    };

    // parsing all parameters according to the list above is sufficent
    c = getopt_long_only(argc, argv, "", long_options, &option_index);

    // no more options to parse
    if (c == -1) break;

    // unrecognized option
    if (c == '?') {
      Usage(argv[0]);
      return 0;
    }

    // dispatch the given options
    switch(option_index) {
    // h, help
    case 0:
    case 1:
      Usage(argv[0]);
      return 0;
      break;

    // d, device
    case 2:
    case 3:
      if (strlen(optarg) == 1)
      {
        char tmp[20];
        sprintf(tmp, "/dev/ttyUSB%s", optarg);
        dev_name = strdup(tmp);
      }
      else
        dev_name = strdup(optarg);
      break;

    default:
      Usage(argv[0]);
      return 0;
    }
  }
#endif

  // Initialize PortHandler instance
  // Set the port path
  // Get methods and members of PortHandlerLinux or PortHandlerWindows
  dynamixel::PortHandler *portHandler = dynamixel::PortHandler::getPortHandler(dev_name);

  // Open port
  if (portHandler->openPort())
  {
    printf("Succeeded to open the port!\n\n");
    printf(" - Device Name : %s\n", dev_name);
    printf(" - Baudrate    : %d\n\n", portHandler->getBaudRate());
  }
  else
  {
    printf("Failed to open the port! [%s]\n", dev_name);
    printf("Press any key to terminate...\n");
    getch();
    return 0;
  }

  char    input[128];
  char    cmd[80];
  char    param[20][30];
  int     num_param;
  char    *token;
  uint8_t dxl_error;

  while(1)
  {
    printf("[CMD] ");
    fgets(input, sizeof(input), stdin);
    char *p;
    if ((p = strchr(input, '\n'))!= NULL) *p = '\0';
    fflush(stdin);

    if (strlen(input) == 0) continue;

    token = strtok(input, " ");

    if (token == 0) continue;

    strcpy(cmd, token);
    token = strtok(0, " ");
    num_param = 0;
    while(token != 0)
    {
      strcpy(param[num_param++], token);
      token = strtok(0, " ");
    }

    if (strcmp(cmd, "help") == 0 || strcmp(cmd, "h") == 0 || strcmp(cmd, "?") == 0)
    {
      Help();
    }
    else if (strcmp(cmd, "baud") == 0)
    {
      if (num_param == 1)
      {
        if (portHandler->setBaudRate(atoi(param[0])) == false)
          fprintf(stderr, " Failed to change baudrate! \n");
        else
          fprintf(stderr, " Success to change baudrate! [ BAUD RATE: %d ]\n", atoi(param[0]));
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
        continue;
      }
    }
    else if (strcmp(cmd, "exit") == 0)
    {
      portHandler->closePort();
      return 0;
    }
    else if (strcmp(cmd, "scan") == 0)
    {
      Scan(portHandler, packetHandler1, packetHandler2);
    }
    else if (strcmp(cmd, "ping") == 0)
    {
      uint16_t dxl_model_num;

      if (num_param == 0)
      {
        fprintf(stderr, " Invalid parameters! \n");
        continue;
      }

      fprintf(stderr, "\n");
      fprintf(stderr, "ping Using Protocol 1.0\n");
      for (int i = 0; i < num_param; i++)
      {
        if (packetHandler1->ping(portHandler, atoi(param[i]), &dxl_model_num, &dxl_error) == COMM_SUCCESS)
        {
          fprintf(stderr, "\n                                          ... SUCCESS \r");
          fprintf(stderr, " [ID:%.3d] Model No : %.5d \n", atoi(param[i]), dxl_model_num);
        }
        else
        {
          fprintf(stderr, "\n                                          ... FAIL \r");
          fprintf(stderr, " [ID:%.3d] \n", atoi(param[i]));
        }
      }
      fprintf(stderr, "\n");

      fprintf(stderr, "\n");
      fprintf(stderr, "ping Using Protocol 2.0\n");
      for (int i = 0; i < num_param; i++)
      {
        if (packetHandler2->ping(portHandler, atoi(param[i]), &dxl_model_num, &dxl_error) == COMM_SUCCESS)
        {
          fprintf(stderr, "\n                                          ... SUCCESS \r");
          fprintf(stderr, " [ID:%.3d] Model No : %.5d \n", atoi(param[i]), dxl_model_num);
        }
        else
        {
          fprintf(stderr, "\n                                          ... FAIL \r");
          fprintf(stderr, " [ID:%.3d] \n", atoi(param[i]));
        }
      }
      fprintf(stderr, "\n");
    }
    else if (strcmp(cmd, "bp") == 0)
    {
      if (num_param == 0)
      {
        std::vector<unsigned char> vec;

        int dxl_comm_result = packetHandler2->broadcastPing(portHandler, vec);
        if (dxl_comm_result != COMM_SUCCESS) packetHandler2->printTxRxResult(dxl_comm_result);

        for (unsigned int i = 0; i < vec.size(); i++)
        {
          fprintf(stderr, "\n                                          ... SUCCESS \r");
          fprintf(stderr, " [ID:%.3d] \n", vec.at(i));
        }
        printf("\n");
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "wrb1") == 0 || strcmp(cmd, "w1") == 0)
    {
      if (num_param == 3)
      {
        Write(portHandler, packetHandler1, atoi(param[0]), atoi(param[1]), 1, atoi(param[2]));
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "wrb2") == 0 || strcmp(cmd, "w2") == 0)
    {
      if (num_param == 3)
      {
        Write(portHandler, packetHandler2, atoi(param[0]), atoi(param[1]), 1, atoi(param[2]));
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "wrw1") == 0)
    {
      if (num_param == 3)
      {
        Write(portHandler, packetHandler1, atoi(param[0]), atoi(param[1]), 2, atoi(param[2]));
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "wrw2") == 0)
    {
      if (num_param == 3)
      {
        Write(portHandler, packetHandler2, atoi(param[0]), atoi(param[1]), 2, atoi(param[2]));
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "wrd2") == 0)
    {
      if (num_param == 3)
      {
        Write(portHandler, packetHandler2, atoi(param[0]), atoi(param[1]), 4, atoi(param[2]));
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "rdb1") == 0)
    {
      if (num_param == 2)
      {
        Read(portHandler, packetHandler1, atoi(param[0]), atoi(param[1]), 1);
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "rdb2") == 0)
    {
      if (num_param == 2)
      {
        Read(portHandler, packetHandler2, atoi(param[0]), atoi(param[1]), 1);
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "rdw1") == 0)
    {
      if (num_param == 2)
      {
        Read(portHandler, packetHandler1, atoi(param[0]), atoi(param[1]), 2);
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "rdw2") == 0)
    {
      if (num_param == 2)
      {
        Read(portHandler, packetHandler2, atoi(param[0]), atoi(param[1]), 2);
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "rdd2") == 0)
    {
      if (num_param == 2)
      {
        Read(portHandler, packetHandler2, atoi(param[0]), atoi(param[1]), 4);
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "r1") == 0)
    {
      if (num_param == 3)
      {
        Dump(portHandler, packetHandler1, atoi(param[0]), atoi(param[1]), atoi(param[2]));
      }
      else{
        fprintf(stderr, " Invalid parameters! \n");}
    }
    else if (strcmp(cmd, "r2") == 0)
    {
      if (num_param == 3)
      {
        Dump(portHandler, packetHandler2, atoi(param[0]), atoi(param[1]), atoi(param[2]));
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "reboot2") == 0 || strcmp(cmd, "rbt2") == 0)
    {
      if (num_param == 1)
      {
        int dxl_comm_result = packetHandler2->reboot(portHandler, atoi(param[0]), &dxl_error);
        if (dxl_comm_result == COMM_SUCCESS)
        {
          if (dxl_error != 0) packetHandler2->printRxPacketError(dxl_error);
          fprintf(stderr, "\n Success to reboot! \n\n");
        }
        else
        {
          packetHandler2->printTxRxResult(dxl_comm_result);
          fprintf(stderr, "\n Fail to reboot! \n\n");
        }
      }
      else
      {
          fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "reset1") == 0 || strcmp(cmd, "rst1") == 0)
    {
      if (num_param == 1)
      {
        int dxl_comm_result = packetHandler1->factoryReset(portHandler, atoi(param[0]), 0x00, &dxl_error);
        if (dxl_comm_result == COMM_SUCCESS)
        {
          if (dxl_error != 0)
            packetHandler1->printRxPacketError(dxl_error);
          fprintf(stderr, "\n Success to reset! \n\n");
        }
        else
        {
          packetHandler1->printTxRxResult(dxl_comm_result);
          fprintf(stderr, "\n Fail to reset! \n\n");
        }
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else if (strcmp(cmd, "reset2") == 0 || strcmp(cmd, "rst2") == 0)
    {
      if (num_param == 2)
      {
        int dxl_comm_result = packetHandler2->factoryReset(portHandler, atoi(param[0]), atoi(param[1]), &dxl_error);
        if (dxl_comm_result == COMM_SUCCESS)
        {
          if (dxl_error != 0) packetHandler2->printRxPacketError(dxl_error);
          fprintf(stderr, "\n Success to reset! \n\n");
        }
        else
        {
          packetHandler2->printTxRxResult(dxl_comm_result);
          fprintf(stderr, "\n Fail to reset! \n\n");
        }
      }
      else
      {
        fprintf(stderr, " Invalid parameters! \n");
      }
    }
    else
    {
      printf(" Bad command! Please input 'help'.\n");
    }
  }
}
```
